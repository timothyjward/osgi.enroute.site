---
title: Resolving - OSGi's Best Kept Secret?
layout: toc-guide-page
lprev: 110-What-is-enRoute.html
lnext: 300-declarative-services.html  
summary: Resolving is one of the cornerstones of OSGi, but what actually is going on?
author: enRoute@paremus.com
sponsor: SMA 
---

The OSGi Framework uses the Resolver to:

* _wire_ together a given set of bundles 
* _select_ the set of required bundles from a population of bundles in one or more local or remote repositories. 

## Modularity and Dependencies  

As soon as you create Modules, you create a dependency management problem.

Realising this, and that the same dependency problem occurs at each structural layer of a software solution (e.g. Resources, Bundles, Services), the OSGi Alliance has evolved, through extensive experience, a generic Requirements-Capabilities [dependency management model](https://osgi.org/hudson/job/build.core/lastSuccessfulBuild/artifact/osgi.specs/generated/html/core/framework.module.html#framework.module.dependencies). 

This model consists of a small number of primitive concepts:

* **Artifact** (a.k.a _Thing_) - For OSGi the primary Artifact is the OSGi Bundle (a JAR file); but other  examples of Artefacts include software Certificates, or physical components such as a secure USB key store.

* **Environment** - The runtime Environment within which an Artefact may be installed: i.e. a physical host, a container, or an OSGi framework.

* **Resource** - A formal description of the Artifact specifying what that Artefact can contribute to the host Environment (its _Capabilities_) and what it needs from the Environment to function (its _Requirements_).

* **Namespace** – Capabilities and Requirements are defined in appropriate [namespaces](https://osgi.org/specification/osgi.core/7.0.0/framework.namespaces.html); every _Requirement_ belongs to a namespace and can only require _Capabilities_ in the same namespace.

* **Capability** - Describes a feature or function of the Resource when installed in the Environment. A Capability has a _type_ (specifying namespace) and a set of key/value  _properties_. Properties are key value pairs, where the keys are strings and values can be scalars or collections of `String`, `Integer`, `Double`, and `Long`.

* **Requirement** - Specifies a Capability needed in an Environment. A Requirement consists of a type and an _OSGi filter_ expressed as an LDAP expression. A Requirement can be _mandatory_ or _optional_.

* **Directives** – For additional flexibiity and backwards compatibility, each namespace can define additional directives. 

**Resolving** is then the process of constructing a composite application from a list of _initial Requirements_, a description of the Environment's _Capabilities_, and one or more _repositories_ with available _Resources_: each Resource specifying the location of its associated Artefact. 

![image](https://user-images.githubusercontent.com/200494/31130842-cf5299d2-a858-11e7-907c-d6cb43954501.png)

The _Resolution_ process uses the list of initial _Requirements_ to find _Resources_ any of the defined _Repositories_ that provide the required _Capabilities_. As selected _Resources_ may have their own _Requirements_, the _Resolver_ process is recursive. 

A _Resolver_ will find either a solution consisting of a set of _Resources_ where all _Requirements_ are satisfied, or that there is no solution.


## Resolving & Repositories 

A _Repository_ is a collection of _Resources_. 

Only the _Resources_ contained within the set of _Repositories_ (one or more) provided to the resolver can be considered during the resolution process: i.e. the resolution process is _scoped_ by these repositories. 

One might consider a single large repository with all possible resources (e.g.  Maven Central), or alternatively a number of tightly scoped respositories, one per application with mininal diversity of resources. 

Each approach has its drawbacks: 
* As resolving is an NP complete problem, resolution times quickly become long when many alternatives in a single large repository need to be examined.
* However, without appropriate Operationl tooling and / or runtime platform, many small tightly scoped repositores - while in principle a good idea - can become an management burden.
Hence usually a small number of _curated_ repositories, each aligned to each Organisational business unit, is usually a good compromise.

The relationship between Repositories and the Resolution process is shown: 

![Resolving](https://user-images.githubusercontent.com/200494/31221580-73596198-a9c4-11e7-8d11-1e1fe7e37199.png)

* **Repository** – A Repository is a collection of resources. Repositories can be represented in many different ways as will be discussed later.
* **Resolver** – The Resolver is the entity that takes a set of _initial requirements_, _system capabilities_, a set of repositories, and produces a _resolution_. 
* **Resolution** – A resolution is a _wiring_ of resources. It provides detailed information about what requirements are matched with what capabilities from the included resources.

An important variable in the resolving process is `effective` which defines the _effectiveness_ under which the resolve operation is performed. The resolver will only look at requirements that it deems _effective_. The default effectiveness is `resolve`. The effectiveness `active` is a convention commonly used for situations that do not need to be resolved by the OSGi framework but are relevant in using the resolver for assembling applications. 


## Well Constructed Bundles

For an OSGi  bundle to be a **good citizen** it needs to comply with the following rules:

* **Minimise Dependencies** – Every dependency has a cost. Always consider if a dependencies is _really_ necessary.
* **API Driven** – Always, always, always separate API and implementation.
* **Service Oriented** – The best dependencies are service oriented dependencies. Services make the coupling between modules explicit and allow different implementations to provide the same service.
* **Package Imports** – A Bundle can either Require another explicitly named Bundle (the Maven model), or rather specify the Packages Required. As many Bundles may be able to offer the Required Packages, specifying Required Packages results in looser coupled assemblies allowing substituion of implementations.
  * The best imported packages are the ones used for services since they are _specification only_.
  * Second best are _libraries_. Library packages have no internal state nor use statics.
  * Importing implementation packages for convenience is usually deemed bad practice because it often indicates that the decomposition in bundles is not optimal.
* **Import the Exports** – If a package is exported **and** used inside the same bundle then the exported package should also be imported. This allows the resolver more leeway.

Unfortunately not all publicly avaliable OSGi bundles follow these best practices.


## Managing Repositories

For reasons explained, Maven Central is far too huge for us to consider resolving against, so we definitely need curated sources of bundles that we can resolve against, ideally without adding too much management overhead. When using Maven it turns out that a POM file is actually a pretty good source of bundles. So instead of searching the whole of Maven Central we can just search the transitive dependency graph defined by the POM file. As long as the POM is well maintained, with properly scoped dependencies, then it functions very well as the basis for a curated repository.  

This is the approached taken by OSGi enRoute. The OSGi enRoute project provides a number of “index” poms which are designed to be used both for convenience in your Maven build, but they are just as useful when you want to resolve your application. You will have seen that in each of the examples there is an _Application_ module which gathers together the application bundles and exports them as a runnable JAR file. If you look a little more closely you will also see that these modules depend on the enRoute indexes, and on the other modules in the application. The reason for this is that application modules use the `bnd-indexer-maven-plugin` to generate OSGi repositories based on the dependencies listed in their poms.

Working in this way reduces the repository management overhead to maintaining the dependencies in your application’s pom file. If any of the other modules in the application change their dependencies then this is automatically reflected in the OSGi repository generated by the indexer. The only time a change is needed to the application project is if a new leaf module (i.e. one that isn’t depended on by other modules in the application) is added to the application.


## Conclusion

The OSGi Resolver is responsible for assembling composite artefacts from selected sets of self-describing OSGi Bundles: so enabling substitution and re-use.

Like Linux packager managers, the OSGi resolver helps shield the complexity associated with assembly from the Developer and the Operator of the modular system. However, unlike Linux package managers, the OSGi resolver is adaptive; meaning it has the ability to re-assess and change acomposite assembly of Bundles over time: providing a basis for structural adaption.

For further details concerning the OSGi Resolver & Repository consult [OSGi Core Release 7 specifications](https://osgi.org/hudson/job/build.core/lastSuccessfulBuild/artifact/osgi.specs/generated/html/core/index.html). 
