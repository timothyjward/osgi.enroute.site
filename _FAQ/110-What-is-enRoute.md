---
title: What is enRoute? 
layout: toc-guide-page
lprev: 100-Why-OSGi.html 
lnext: 200-resolving.html 
summary: The Scope and Objectives of the enRoute project. 
author: enRoute@paremus.com
sponsor: OSGi™ Alliance 
---

enRoute is intended to provide a gentle introduction to those who wish to start building modular systems. 

* enRoute demonstrates the quick and simple creation of stand-alone OSGi applications.
    * Providing basic guidance for using R7 specifications.
    * Providing a simple set of dependencies for using OSGi R7 Reference Implementations
* enRoute takes an opinionated approach focusing on 
    * Declarative Services 
    * and a Maven based tool chain.
* All enRoute examples are based on the latest OSGi Specifications and Reference Implementations.

enRoute is not intended to be encyclopaedia for experienced OSGi developers, nor is enRoute aligned with any particular open source initiative. enRoute also stops well short of explore advanced OSGi topics such as asyncronous distributed systems, adaptive assembles or reactive behaviours; but armed with a solid understanding of the basics, enRoute enables you to build expertise and progress onto these exciting topics.    

## Why is enRoute needed?

The [TIOBE Index for January 2018](https://www.tiobe.com/tiobe-index/) yet again places Java as the industries leading programming language, a position broadly held since 2003. Yet despite this, and despite an increasing understanding in the industry that Modularity is essential for achieveing economically sustainable software' systems, the average Java developer is not an OSGi developer. 

There are several reasons for this:

* As demonstrated by DropWizard & Spring Boot - software industry is focus on creating stand-alone, production-grade Applications that you can **"just run"**. A simple message for new developers. However these frmeworks avoid the hard maintainability and evolvability issues that plague our industry.
* Since it's inception OSGi concepts have continued to evolve. This is a testement to the power of OSGi, but makes keeping an OSGi start-tutorial current on on going challenge / moving target.
* The internet - which never forgets - is littered with outdated OSGi tutorials and articles.
* OSGi developer tooling exposed too much of the internals to a new Developer: yes OSGi was hard.

Today with OSGi R7, enRoute completely removes the barrier to developing modular, maintainable software systems. There is no execuse for creating a legacy of technical debt.

![Lowering the barriers](/img/book/why-enroute.png)

## Relationship to previous version of enRoute?

The OSGi Allaince, realising the need to create a simple on-ramp for new Developers, engaged Peter Kriens to create a simple experience for a Developer new to OSGi; the results of which - enRoute1.0 - [was announced in 2015](http://blog.osgi.org/2015/10/osgi-enroute-10.html). 

In addition, to help shape some of the specifications in R7, enRoute1.0 provided a context against which OSGi members could discuss next steps.  The present enRoute project is the result of those discussions and experiences. 

## Looking ahead

enRoute will be continued to be enhanced with simple examples that demonstrated best use of OSGi Allaince R7 Specifications. Looking ahead enRoute will continue to track the OSGi Specification process, with new enRoute examples being created to demonstrate R8 capabilities and beyond.


