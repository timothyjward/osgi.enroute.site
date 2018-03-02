---
title: Resolving - OSGi's Best Kept Secret?
layout: toc-guide-page
lprev: last.html
lnext: next.html
summary: Resolving is one of the cornerstones of OSGi, but what actually is going on?
author: enRoute@paremus.com
sponsor: SMA 
---

The OSGi Framework has always used a resolver to:

* _wire_ together a given set of bundles 
* _select_ this set of required bundles from a much larger population of bundles in one or more repositories. 

Like Linux packager managers, the OSGi resolver helps shield the complexity associated with assembly from the Developer and the Operator of the modular system. However, unlike Linux package managers, the OSGi resolver is adaptive; meaning it has the ability to re-assess and change an assembly of Modules over time.  


## Principles

The OSGi resolving model includes the following entities:

* **Resource** – A resource is a _description_ of an artefact that can be _installed_ in a system. When it is installed, it adds functions to that system. However, before it can be installed it requires certain functions to already be present. A resource can describe a bundle but it could also describe a piece of hardware or a certificate. It is important to realise that a resource is _not_ the artefact, it is a description of the _relation_ between the artefact and the target system. For example, in OSGi the bundle is a JAR file that can be installed in a framework, this is the artefact. A resource describes formally what that bundle can contribute to the system and what it needs from the system.
* **Capability** – A capability is a description of a resource's contribution to the system when its artefact is installed. A capability has a _type_ and a set of _properties_. The type is defined by name and is therefore called the _namespace_. The properties are key value pairs, where the keys are strings and values can be scalars or collections of `String`, `Integer`, `Double`, and `Long`. 
* **Requirement** – A requirement represents the _needs_ of an artefact when it is installed in the system. Since we describe the system with capabilities, we need a way to assert the properties of a capability in a given namespace. A requirement therefore consists of a type and an _OSGi filter_. The type is the same as for the capabilities, it is the namespace. The filter is constructed from a powerful filter language based on LDAP. A filter can assert expressions of arbitrary complexity. A requirement can be _mandatory_ or _optional_.
* **Namespace** – Since a requirement can match any set of properties it is necessary to scope the capabilities it can be asserted against. This is achieved by giving a requirement and a capability a namespace. A requirement can only match a capability when the capability has the same namespace.

Resolving is then the process of constructing a composite application from a list of _initial requirements_, a description of the _system capabilities_, and one or more _repositories_. 

![image](https://user-images.githubusercontent.com/200494/31130842-cf5299d2-a858-11e7-907c-d6cb43954501.png)

The resolution process uses the list of initial _Requirements_ to find resources in the repository that provide the required _Capabilities_. As selected resources may have their own _Requirements_, the resolver process is recursive. 

A _resolver_ will find either a solution consisting of a set of resources where all requirements are satisfied, or that there is no solution.


## Resolving & Repositories 

A _repository_ is a collection of resources. Only the resources contained within the set of repositories (one or more) provided to the resolver can be considered during the resolution process: i.e. the resolution process is _scoped_ by these repositories. 

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


### Examples of Repositories

OSGi Repositores may take any of the following forms.

* **OSGi XML Format** – The OSGi Alliance specified an XML format for repositories.
* **Maven Central** – Subsets of Maven repositories using a simple text file, a query on Maven Central, or a `pom.xml`. It integrates fully with the local `~/.m2` repository.
* **Eclipse P2** – Eclipse repositories have a web based layout that can be parsed by bnd.
* **File based** – You can store bundles in a directory structure and use it as a repository.
* **Eclipse Workspace** – In Bndtools, all generated bundles in a workspace are directly available to the resolver.


### Managing Repositories

??

A repository generally represents a _release_ of a software product. For example, in OSGi enRoute, the OSGi Alliance has provided a _base API_ bundle. This bundle contains all the APIs of OSGi core, OSGi compendium, and an OSGi enRoute release. There is also an OSGi enRoute _distro_ for each release. The distro is a repository that provides access to implementations for the base API. The OSGi enRoute distro is maintained in a Maven POM. This POM lists the exact versions of a release. Many developers publish an OSGi Repository XML to allow the resolver access to the metadata of their bundles, for example [Knopflerfish](http://www.knopflerfish.org/releases/4.0.1/repository.xml) produces an OSGi XML Repository with all their bundles.

This model is in contrast with Maven Central and most other Maven repositories. These repositories are designed to contain everything that was ever released. An OSGi repository is more the content of a specific release. Since it generally only contains a specific release, it will disallow the resolver to use any unwanted resources.

??

### Workspace Repository

??

A natural repository is the bnd _workspace_ since a workspace is a collection of _related_ projects. Since these projects are build together it is trivial to keep them compatible. The metadata burden can be mitigated by sharing metadata between these related projects. For example, in bnd all bundles share the same version. Although this might sometimes release unchanged bundles under a newer version, the only cost is a bit of disk space. A small price to pay for an otherwise very error prone manual process. The Gradle build can release to a Nexus repository and automatically generate an index that can be used as repository.

??

## How to Write Resolvable Bundles

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

## Conclusion

The resolver is responsible for assembling composite artefacts from selected sets of self-describing OSGi Bundles: these behaviours enabling both  re-use and strutural evolvability.

For further details concerning the OSGi Resolver & Repository consult [OSGi Core Release 7 specifications](https://osgi.org/hudson/job/build.core/lastSuccessfulBuild/artifact/osgi.specs/generated/html/core/index.html). 

