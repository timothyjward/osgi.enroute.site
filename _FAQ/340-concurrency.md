---
title: DS and Dynamism 
summary: How Declarative Services and Dynamism 
layout: toc-guide-page 
---


## Guarantees from DS

In many aspects DS is a dependency injection (DI) engine similar to Spring, Guice, or
CDI. However, in contrast with these DI engines DS components have a life cycle.
This adds the dimension of _time_ to the component, something that is utterly lacking in
any other DI engine. DS components are dynamic.

Though this extra dimension provides tremendous power it clearly comes with the
burden that things do not only get initialized, it is also possible that things go
away.

DS goes out of its way to hide that complexity. The way DS achieves this is through
very strong guarantees. DS absorbs a tremendous amount of complexity so that you
don't have to worry about it. However, many a developer writes their code defensively
and overly locks/synchronizes parts. So let's see what we can rely on.

### Ordering

DS provides a very strict ordering. This ordering implies that there is a very clear
happens before relationship between the different phases. There is no gain using volatile
variables or other synchronization constructs between these phases because this is
already been done by DS. That is, if you set an instance variable in the constructor
then there is a full guarantee that this variable is visible in the bind
or activate methods even if these methods, as is allowed, were called on different threads.

The following is the guaranteed ordering that a DS component can observe:

1. Constructor – DS will always create a new object, it will **never** reuse an existing object.
2. Bind – The bind methods or field injections are called in alphabetical order when using annotations.
   (Though dynamic methods or field injections can of course be called at any time.)
3. Activate – Only if all static reference methods and field injections are called is the activate method called. If this method does
   not throw an exception, it is guaranteed that the deactivate will be called. If
   an exception is thrown the following phases are not executed.
4. Active – During the active phase the following methods can be called in any order from
   any thread and in parallel:
   * Any methods of the registered services
   * A modified methods that dynamically takes the modified configuration properties
   * Any of the updated reference methods if defined
5. Deactivate – Clean up
6. Unbinds – And unbind methods are called
7. Release of object – DS will release the object so that no longer any references are held
8. Finalize – Java garbage collects the object

Lazy services are registered before their constructor is called. The initialization of the
DS component will take place when the service is used for the first time. However, this
should not be observable by the component itself.

## Static References

The default and simplest model of DS is to use `static` references. If a component only has
static references then it never sees any of the OSGi dynamics. This means that
with the given ordering there is no need to use volatile or other
synchronization constructs for static references. (Of course the service methods are
still called from different threads.) Field injection is the most
simple way but bind and the optional unbind methods do not require any synchronization constructs.

Sadly, many developers that start using dynamic references making the grave error of premature optimization. Yes,
you can wring more performance out of a computer making these components dynamic but
it is rarely worth the added complexity. Only go to dynamics with things like the
whiteboard and/or when you measure a significant cost for being static. In almost
all cases the static model where the component is destroyed and then recreated works
extremely well ... really.

Anyway, remember the rules about optimization:

* Do not optimize!
* Only for the experts, do not optimize yet!

## Optional References

Sometimes a component can deliver its functionality even when a reference is
absent. This is then an _optional_ reference. By far the simplest method to
handle this is to make the reference optional by specifying the cardinality:

```java
	@Component(service=ReluctantOptionalReference.class)
	public class ReluctantOptionalReference {

		@Reference(cardinality=ReferenceCardinality.OPTIONAL)
		Foo reluctantOptionalReference;

	}
```

However, this is a _static_ reference. This implies that the component is started
regardless of the presence of Foo. If Foo happens to be there then it is injected
otherwise the field remains `null`. This model is called _reluctant_.

Unfortunately, this means we miss the Foo service when it is registered a few
nanoseconds later. Since the static model has so many advantages there is an option
to reconstruct the component when this reference finds a candidate. This
is the _greedy_ mode:

```java
	@Component(service=GreedyOptionalReference.class)
	public class GreedyOptionalReference {

		@Reference(
			cardinality=ReferenceCardinality.OPTIONAL,
			policyOption=ReferencePolicyOption.GREEDY)
		Foo greedyOptionalReference;
	}
```

DS will now reconstruct the component when there is a _better_ candidate for
`foo`. Clearly any candidate will beat no candidate but what means better in the
case that we already have `foo`?

When multiple candidates are available DS will sort them by ranking. Services
with a higher _ranking_ are deemed _better_. Service ranking is indicated by a
property called `service.ranking`. It is an integer, higher is better.

One of the advantages of the static model is that in your activate method all
the way to your deactivate method your visible world won't change.

The previous examples were still static because none of the references changed
between the `activate` and `deactivate` phase. The greedy policy option achieved
its replacement by reconstructing the component. This is acceptable in most
cases but sometimes the component does not want to die for the sake of an
optional reference. In that case we can handle the situation _dynamically_.

By far the easiest solution is to mark the field as _volatile_. **A volatile
field will automatically get marked as `policy=DYNAMIC`**.

```java
	@Component(service=DynamicOptionalReference.class)
	public class DynamicOptionalReference {

		@Reference(cardinality=ReferenceCardinality.OPTIONAL)
		volatile Foo dynamicOptionalReference;
	}
```

This is simple but there is an obvious price. The following bad code shows
a common (but horrible) pattern that people use to use `foo`:

```java
	if ( foo != null ) // BAD!
		foo.bar();
```

This innocuous looking code is actually a Null Pointer Exception in the waiting.
A better way is to do:

```java
	Foo foo = this.foo;
	if ( foo != null )
		foo.bar();
```

By using a local variable we guarantee that the check (is `foo null`?) is using the
same object as the one we will call `bar()` on. This is a very cheap form of
synchronization.


## But What If The Service Disappears?

A common question that now appears is: 'What if the service goes away?'. You
could be in the process of calling a service when it becomes unregistered. Sometimes
this will cause the service to throw exceptions sometimes you get a Service Exception.

In general it is one of those cases where 'shit happens'. In a concurrent environment
like Java it is possible to end up in a sad place. Your code should always be prepared
to accept exceptions when you call other services. This does not mean you should catch
them, on the contrary. It is much better to forward the exceptions to the caller
so that they do not unnecessarily get wrapped up in wrapping exceptions and lose the
original context.

In almost all cases there is a top level function that initiated your request. It
is this function that has the responsibility to make sure the overall system
keeps working regardless of failures. This kind of robustness code is extremely
difficult to get right and should **never** be mixed with application code.

However, any locks you hold in a method should be released and resources should be closed. Any code
you call can cause exceptions. Relying on other code and not handling their exceptions
(even if they are not declared) is living a very optimistic, naive, and likely short
professional life.

## Tracking Multiple Services

If you use a whiteboard pattern or other listener like model then in general
you want to use dynamics. The reason is that you have _multiple_ references
and building and destroying the component at every change in the set of services
we're interested in (the _tracked_ services) becomes expensive.

By far the easiest method is to use field injection of a list of services.
If you make this field `volatile` then DS will inject a new list whenever the
set of tracked services changes.

```java
	@Component(service=SimpleList.class)
	public class SimpleList {

		@Reference
		volatile List<Foo>		dynamicFoos;
	}
```

However, there are many scenarios where the component must interact with the bind
and unbind of the references. The most common way is then to create a bind
and unbind method.

```java
	@Component(service = DynamicBindUnbind.class)
	public class DynamicBindUnbind {

		final List<Foo> foos = new CopyOnWriteArrayList<>();

		@Reference(
			cardinality = ReferenceCardinality.MULTIPLE,
			policy = ReferencePolicy.DYNAMIC)
		void addFoo(Foo foo) {
			foos.add(foo);
		}

		void removeFoo(Foo foo) {
			foos.remove(foo);
		}
	}
```

In this example we use a `CopyOnWriteArrayList`. This is a so called _non-locking_
object. Though it is perfectly safe to use in a concurrent environment it will not
use locks and any iteration over that list is guaranteed not to fail. Many list
types in Java will fail with a Concurrent Modification Exception if you add/remove
a value while another thread iterates. A `CopyOnWriteArrayList` won't. It has a simple
trick for this. Instead of adding a new element to the list, it replaces the
internal storage array with a new array. Iterations iterate over this array. Though
this means the iteration can visit stale objects, the length of the array and its
content will never change. Each iteration will be bound to a single generation.
