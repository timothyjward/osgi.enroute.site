---
title: Why OSGi? 
layout: toc-guide-page
lprev: 450-Designing-APIs.html 
lnext: 110-What-is-enRoute.html 
summary: The foundations for Economical Sustainable Software Systems  
author: enRoute@paremus.com
sponsor: OSGi™ Alliance 
---

Whether an emergent Natural system, or one created by Homosapiens, modularity is exploited to manage Complexity. The reason? Decoupled Modules are simpler to change and evolve relative to the equivalent functional monolith. Modules may also be used in different combinations and contexts, so encouraging re-use. 

From a business perspective these characteristics translate to maintainable, adaptable, and so **economically sustainable** software systems.


## OSGi History

In 1998 a number of software companies joined together to develop a software standard for Java based residential gateways. Out of this effort grew the OSGi Alliance with its Core Framework and Service specifications. 

The approach taken by the OSGi Alliance was to leverage Java's existing language structures. 
* Since Java Packages have access rules it is possible to shield the internal implementation details by making Packages private: the shielding of internal implementation details being one of the **essential characteristc** of any Modularity system. 
* For the deployment artefact - the Java JAR file would be used. Rather than create new access control mechanisms, the existing Package boundaries would be enforced. 

These efforts by the OSGi Alliance resulted in the OSGi **Bundle**, the OSGi framework, the Requirements / Capabilities dependency model and the Resolver / Assembly process.

While historically Java centric, the OSGi Requirement-Capability dependency model is language neutral and is increasingly being used to address general dependency management problems. 


## Relationship to Components?

Those new to modularity can find the difference between _components_ and bundles (modules) initially confusing.

Quite simply:
* **OSGi Bundle**: A physically deployable artefact, that is the atomic unit of structural modularity. A Bundle may contain code and/or configuration.
* **Component**: A logical grouping of code that represents a unit of functionality.

A 1:1 relationship could exist between a _Component_ and a Bundle, however good design practices dictate that tightly coupled _Components_ - meaning Components that need to change together - should be colocated in the same Bundle (i.e. high Cohesion). A simple example is provided by microservices [DAO Impl](../tutorial/030-tutorial_microservice.html#the-microservice-dao-impl), where the relevant implementations for data access layer are placed in the same OSGi bundle. Conversly, _Components_ that are loosely coupled should reside in their own respective Bundles.

Some typical Component-Bundle scenarios include:  
* **Library**: A collection of related Components, each with a public API and no internal state. A library many be used by many different Client components, in different runtime contexts, and so a Library is a good candidate for being a self-contained Bundle.

* **Stateful Component**: These provide a system wide function, shared by all other components. An example is again provided by the Microservice [DAO Impl](../tutorial/030-tutorial_microservice.html#the-microservice-dao-impl). 

* **Abstract Component** An abstract component separates its API from its implementation; placing these in seperate bundles. Why do this? The abstract API Bundle defines the functional contract which must be met by an implementation Bundle; while deferring the selection of the implementation Bundle to the assembly process: e.g. specified by different persons/organizations: the [DAO API](../tutorial/030-tutorial_microservice.html#the-microservice-dao-api) bundle is an example of this; it de-coupling the data-layer implementation [DAO Impl](../tutorial/030-tutorial_microservice.html#the-microservice-dao-impl) from the client REST services. 

* **Extender Component** The purpose of extender components is to simplify the implementation of other components by taking care of boilerplate code, allowing the other components to become smaller and more concise. For example, OSGi Declarative Services removes a myriad of dependency handling and configuration details from a component by inspecting an XML file in its bundle. Since the component code has no traces of these aspects, it becomes more readable and more predictable.


## Which Component Model?

As one would expected from a well designed ecosystem, the OSGi module layer is agnostic to the Dependency Injection (DI) / Java component framework used and you find examples of OSGi used with Spring, CDI & Guice. However while alternative DI frameworks can work; Declarative Services is the only component framework that leverages the dynamicity of OSGi while also providng simple abstractions, and so is strongly recommended by the OSGi Alliance. 
