---
title: Semantic Versioning 
layout: toc-guide-page
lprev: 200-resolving.html
lnext: 300-declarative-services.html  
summary: Why Semantic Versioning is essential for Maintainability   
author: enRoute@paremus.com
sponsor: OSGi Allaince 
---

In a graph of interconnected software components how do we know if a node changes? Then, how do we know if the composite assembly will still function as intended given this change?

Versioning of the nodes addresses the first problem; semantic versioning of nodes addresses both problems.

## Change and Semantic Versioning

By applying simple versioning we can see that changes have occurred; however we do not understand the impact of these changes. However if we use semantic versioning, then the potential impact of this change can also be communicated.

Semantic versioning in OSGi is achieved in the following manner:

* A _version_ has 4 parts: `major 1 minor 1.1 micro 1.1.1 qualifier 1.1.1.qualifier`
* A _version policy_ must is defined: i.e. an agreed semantic interpretation. A change in: 
    * major ➞ a breaking change 
    * minor ➞ a backward compatible changes 
    * micro ➞ a bug fix (no API change) 
    * qualifier ➞ a new build 
* Export-Package (or Capabilities) - Here the decision was taken to support a single export version (i.e. `Export-Package: com.acme.foo; version=1.0.2`). So a  non-breaking to `com.acme.foo` might be represented by a version change 1.0.2 ➞ 1.1.0; where as the subsequent version change 1.1.0 ➞ 2.0.0 represents a breaking change.
* Import-Package (or Requirements) now specified the range of acceptable Export-Packages (Capabilities) versions (i.e. Import-Package: com.acme.bar; version="[1,2)"). Square brackets ‘[‘ and ‘]‘ are used to indicate inclusive and parentheses ‘(‘ and ‘)‘ to indicate exclusive. Hence a range [1.0.0, 2.0.0) means any Capability with version at or above  1.0.0 is acceptable up to, but not including 2.0.0. 

In the this example we know that `com.acme.bar` will work with `com.acme.foo 1.0.2 & 1.1.0`; however we also know that we have a breaking change if we move to com.acme.foo 2.0.0 without without appropriate updates to `com.acme.bar`.
{: .note } 


## Dont Aggregate Dependencies!

OSGi versioning is on packages, not on bundles. The reasoning for this is simply that Bundles are an ‘‘aggregate’’ artefact and so must move as fast as the fastest moving exported packages they contain. 

Letss pretend for the moment that this is not the case, and that we only version Bundles.

Consider a scenario where a bundle contains just two exported packages `foo` and `bar`. A change is applied where,
* `foo` is not changed
* `bar` has a major change.
 
To reflect this the bundle version must also have a major change. However this now requires an unnecessary updates for bundle that only have an actual dependence on `foo`. Aggregating dependencies in this way increases the fan out of the _transitive dependencies_; rapidly resulting in brittle composite systems where where all of the constituent parts must be simultaneously updated.

Note that exactly the same problems applies with respect to REST based Microservices and relying on container image versions: yet another reason to use OSGi as the basis for modular Microservices.


## Further reading

The OSGi Alliance have been leading and educating the industry in the use of [semantic versioning](http://www.osgi.org/wiki/uploads/Links/SemanticVersioning.pdf) and its importance for building maintainable software systems for many years. Further information on Semantic Versioning and its automatic management by Bndtools can be found at [Bndtools documentation](http://bnd.bndtools.org/chapters/170-versioning.html) 

More recently, an attempt to publicize the general value of Semantic Versionin  at semver.org, which basically aims to do the same, but outside of OSGi

