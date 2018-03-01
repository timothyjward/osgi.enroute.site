---
title: What is a Data Transfer Object (DTO)?
summary: OSGi started to heavily use DTO's since R5. DTOs do not look very object oriented, this application note provides the reasoning why DTOs .
layout: toc-guide-page 
---

A Data Transfer Object is used to represent the state of a related runtime object in a form suitable for easy transfer to some receiver. The receiver can be in the same Java VM but is more likely in another process or on another system that is remote. All Data Transfer Objects are easily serializable having only public fields of a limited set of type. DTO's also provide a normalised  internal data representation which, via the Converters introduced in R7, may be mapped to, for example JMX, JSON, YAML representations, or visa-versa. 


## A DTO's structure

Data Transfer Objects are public (static) classes with no methods, other than the compiler supplied default constructor, having only public fields limited to the easily serializable types: i.e. A DTO is equivalent to a _struct_ in C.
For example:

        public class BundleDTO extends DTO {
                public long   id;
                public long   lastModified;
                public int    state;
                public String symbolicName;
                public String version;
        }

the _Types_ that can be used in DTOs is limited to:

* Primitive types
* Wrapper classes for the primitive types
* String
* enum
* Version
* Data Transfer Objects
* List
* Set
* Map
* array

The List, Set, Map and array aggregates must only hold objects of the listed types.

Again, as the object graph from a Data Transfer Object must be a tree to simplify serialization and deserialization, the DTO class has some very specific requirements:

* **public** – Only public classes with public members work as DTO. 
* **static** – Fields must **not** be static but inner classes must be static. 
* **no-arg constructor** – To allow instantiation before setting the fields
* **extend** – DTOs can extend another DTO
* **no generics** – DTOs should never have a generic signature, nor extend a class that has a generic signature because this makes serialization really hard.


### Equals and HashCode

The DTO class has no `equals` nor a `hashCode` method. This makes them unsuitable as keys or for use in sets. The reason is that it is not the 
data (the DTO) that defines what is the primary key of the data, it is the actual usage scenario that defines the fields of interest. 
That is, one object can use the `id` field of the object as primary key but another object tracks by `symbolicName` and `version`.

You will therefore often find that DTOs are the values in maps but never a key. One field is then the key.  

It is rare but allowed to add `equals` and `hashCode` methods.


### DTOs and Threads 

A Data Transfer Object is a representation of a runtime object at the point in time the Data Transfer Object was created. Data Transfer Objects do not track state changes in the represented runtime object. Since Data Transfer Objects are simply fields with no method behavior, modifications to Data Transfer Object are inherently not thread safe. Care must be taken to safely publish Data Transfer Objects for use by other threads as well as proper synchronization if a Data Transfer Object is mutated by one of the threads.


## Where next?

The use of DTO's is introduced in the [Microservies Tutorial](../tutorial/030-tutorial_microservice.html) via their use as `Person` and `Address` data objects. 

The org.osgi.dto package defines the basic rules and the abstract base DTO class which Data Transfer Objects must extend. 

For further information see:

* [Data Transfer Objects](https://osgi.org/hudson/job/build.core/lastSuccessfulBuild/artifact/osgi.specs/generated/html/core/framework.dto.html) - link to the DTO Speciofication. 
* [Converter](https://osgi.org/hudson/job/build.cmpn/lastSuccessfulBuild/artifact/osgi.specs/generated/html/cmpn/util.converter.html) - link to the Converter specification

