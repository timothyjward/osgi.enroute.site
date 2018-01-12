---
title: Complex Systems 
summary: How Modularity tames Complexity 
layout: concepts 
lprev: 010-concepts 
lnext: 095-modularity-maturity-model
---

Low-maintenance, highly adaptive software eco-systems are the essential foundations for tomorrow's economically sustainable software eco-systems (i.e. Smart City and Industry4.0). Yet, as DARPA realise, today’s software systems fail to meet these challenges.

> Modern-day software systems, even those that presumably function correctly, have a useful and effective shelf life orders of magnitude less than other engineering artefacts ...

> while an application's lifetime typically cannot be predicted with any degree of accuracy, it is likely to be strongly inversely correlated with the rate and magnitude of change of the ecosystem in which it executes.

(DARPA BRASS -`Building Resource Adaptive Software Systems` - Initiative 2015).

The aim of DARPA BRASS challenge is simple; to create software solutions that avoid obsolescence; to create software eco-systems that are able to run for more than 100 years. Such software eco-systems must be economically sustainable over these extended periods, and to achieve this longevity, these software eco-systems must be able to; cost-effectively adapt and evolve in response to unforeseen changes in environmental conditions, and must be simple to maintain, or more probably, be self-maintaining.

Yet after more than a decade into 'virtualisation', and more recently Containerisation, Microservices, BlockChain & AI; these maintainability and evolvability goals seem more illusive than ever.

Why is this such a challenge?

As payback is not immediate, addressing foundational issues like sustainability and evolvability receives little interest from the software Industry; and yet addressing these issues lies at the heart of any sustainable approch to Smart City, Industry 4.0 and Mobile Edge Computing. Yet, there is a deeper problem. While the inter-related concerns of structural Complexity and Change are challenges shared by all engineering disciplines; it is perhaps the fluidity and rate of change that uniquely differentiates software engineering; aligning it closer to Biological or Social Systems. 

It is perhaps to those disciplines we should look to for guidance?


## Complexity & Software Engineering

The answer to the above question is both Yes and No. 

Current research into Complex Adaptive Systems has much to teach the software industry; however a number of foundational principles were independently discovered by the Software Industry in the 1970's. 

Notable insights into the nature of Complexity within Software systems include:

> _Gall's Law_: A complex system that works is invariably found to have evolved from a simple system that worked. A complex system designed from scratch never works and cannot be patched up to make it work. You have to start over, beginning with a working simple system.

and

> _Lehman’s Law_: As a system evolves, its complexity increases unless work is done to maintain or reduce it.

Regarding the role of Modularity in tackling this problem, [Parnas](https://en.wikipedia.org/wiki/David_Parnas) in his seminal paper [On the Criteria To Be Used in Decomposing Systems into Modules](http://repository.cmu.edu/cgi/viewcontent.cgi?article=2979&context=compsci), [Parnas](https://en.wikipedia.org/wiki/David_Parnas) states... 

> “The major advancement in the area of modular programming has been the development of coding techniques and assemblers which (1) allow one module to be written with little knowledge of the code in another module, and (2) allow modules to be reassembled and replaced without reassembly of the whole system. This facility is extremely valuable for the production of large pieces of code ..."

Parnas goes on to exaplain that...

> the impact of change on a Modular composite system is dictated by _how_ the system was decomposed. If a small change in a single Module causes a cascade of many downstream changes in other modules; then Modules in the system are tightly-coupled. However if the change results in with minimal / no / downstream changes, then the Modules in the System are loosely-coupled.
From the perspect of Software Systems, Parnas concludes

With respect to hierarchy Parnas states...

> The existence of the hierarchical structure assures us that we can "prune" off the upper levels of the tree and start a new tree on the old trunk. If we had designed a system in which the "low level" modules made some use of the "high level" modules, we would not have the hierarchy, we would find it much harder to remove portions of the system, and "level" would not have much meaning in the system.

and finally

> we must conclude that hierarchical structure and "clean" decomposition are two desirable but independent properties of a system structure.

As explained later, runtime Hierarches are actually the result of Modularity and so in this case the two concepts are closely inter-related.    

Further notable insights into the nature of Complexity within distributed Software systems include:

> [_The 8 Fallacies of Distributed Computing_](http://nighthacks.com/jag/res/Fallacies.html)

which despite claims to the contrary are as valid today as when they were first crafted.  


## Complexity, Change and the role of Modularity

All Complex Adaptive Systems (CAS) are composed of a hierarchy of structural layers; each layer internally modular. Complex adaptive systems (CAS), including Ecosystems, Social Systems, Biological cells, and Financial Markets, are characterized by intricate hierarchical arrangements of boundaries and signals: [Signals and Boundaries - John Holland](https://mitpress.mit.edu/books/signals-and-boundaries). The following principles are quite generic and will be familar to software engineers conversent with SOLID / Lean Agile methodologies 

* **Modularity** - Why is Modularity so important? Modularity makes complexity manageable; Modularity enables parallel work; and Modularity is tolerant of uncertainty. By “tolerant of uncertainty” we mean that particular elements of a modular design may be changed after the fact and in unforeseen ways: [Design Rules, Volume 1 The Power of Modularity](https://mitpress.mit.edu/books/design-rules). To successfully implement Modularity, the internal implementation details of a Module must be shielded from the Module's external environment: the external environment being the aggregate of all other modules, the underlying runtime and higher level Services. The only permissible interactions between the Module, and its host `Environment`, is then via the Module’s (in a software context) public Interfaces. Once achieved, the internal implementation details of the Module can be changed at will.

* **Cohesion** - Systems evolve over time. To accomodate structural change Systems must localise the effects of structural chnage by maximising Cohesion: Things that are closely related, that must change in unison, that must evolve together, need to be colocated together within the same Module. Things that evolve at a different rates, and/or could be used separately in different runtime contexts, should be in separate Modules, and these Modules should be `Loosely Coupled` via the published Interfaces that change much more slowly over time. 
The _Principles of Package Cohesion_  summarise these ideas: 
   * Reuse-release equivalence principle (REP) - The unit of reuse is the unit of release.
   * Common-reuse principle (CRP) - Classes that aren’t reused together should not be grouped together.
   * Common-closure principle (CCP) - Classes that change together belong together.

* **Loose Coupling** - To allow substitution and interchangeability Modules must be loosely coupled. Critically this cannot be achieved if Modules refer to each other explicitly by a specific unique name and version. Rather, to enable substitution, Module relationships need to be expressed in terms of the Capabilities they expose to the environment via the public Interfaces, and also what they Require from the environment to run. In the context of software, to allow the impact of changes to be determined, Modules should semantically version these Requirement and Capabilities (see - https://semver.org, also https://www.osgi.org/wp-content/uploads/AgilityandModularity2014v21.pdf). The _Principles of Package Coupling_ provide guidance concerning the inter-dependencies between Loosely-Coupled Modules:
   * Acyclic dependencies principle (ADP) - The dependency structure between packages must not contain cyclic dependencies.
   * Stable-dependencies principle (SDP) - The dependencies between packages should be in the direction of the stability of the packages. A package should only depend upon packages that are more stable than it is.
   * Stable-abstractions principle (SAP) - Packages that are maximally stable should be maximally abstract (i.e. public Module Interfaces). Unstable packages should be concrete (Module implementations). The abstractness of a package should be in proportion to its stability.

* **Structural Hierarchy** - As already explained boundaries encapsulate complexity, and the entities created by these `Boundaries` interact with each other via loosely coupled `Signals`. What is perhaps less well appreciated is that The introduction of Modularity at one structural layer introduces a new higher layer of structural abstraction. This process is iterative; by this we mean that a new boundary can be created to enclose a group of related Modules, thereby creating a super-Module: and so yet another layer of structural abstraction. The following questions apply at each layer of this hierarchy: 
   * Which Modules are required to re-produce the desired functionality for the next sturctural layer?
   * How do these Modules communicate at runtime if co-located, if distrubuted?
   * What is the life-cycle? 
   * How is the Composite entity configured?

The structural OSGi™/Java hierarchy will be familiar to the reader and perfectly illustrates this concept. 

![Modularity and complexity](img/ComplexityModularity3.png)


## Measuring Complexity

Intuitively one might expect software complexity to grow with respect to the number of _links_ (i.e. links being lines of code calling a method or using a field) inside the code base. In the following diagram we see that module A has 9 internal links, hence a reasonable complexity measure might be proportional to 9<sup>2</sup>. Meanwhile for the Modular alternative we see that the combined complexity estimate (Module B and C together) is 3<sup>2</sup> + 6<sup>2</sup>, which is almost half. A more sophisticated approach to quantitatively determining software complexity was proposed by [Halstead in 1977](https://en.wikipedia.org/wiki/Halstead_complexity_measures). Halstead proposed that software metrics should reflect the implementation or expression of algorithms, but be independent of their execution on a specific platform. Halstead appraoch aped Physics' approach to the measurement of invariant properties of matter (like the volume, mass, and pressure of a gas) and the relationships between these propoerties (Boltzman Ideal Gas equation). 

More sophisticated attempts 

![Modularity and complexity](img/ComplexityModularity1.png)  

Qualitative measures of software complexity can be traced back to Scott Woodfield’s research in 1979: see [An Experiment on Unit Increase in Problem Complexity](https://www.computer.org/web/csdl/index/-/csdl/trans/ts/1979/02/01702600.pdf). With the participation of 48 experienced developers, Woodfield conducted a series of experiment to investigate how different types of modularisation and comments relate to programmers' ability to understand, correct and modify programs. Woodfield’s research is summarised by Robert Glass in his book [Facts and Fallacies of Software Engineering](https://en.wikipedia.org/wiki/Robert_L._Glass) sometimes referred to as `Glass’s Law` states:

> For every 25% increase in problem complexity (F), there is a 100 percent increase in complexity (C) of the software solution.

As demonstrated in the blog post [The Equation every Enterprise Architect Should Memorize - Roger Sessions](http://simplearchitectures.blogspot.co.uk/2012/03/equation-every-enterprise-architect.html), this expression can be recast into:

> A Module with b functions is 10<sup>log(C) x log(b) / log(F)</sup> times more complex than a Module that contains just 1 of the functions.

Applying Glass’s complexity measure to the previous example: Module A with 9 functional units gives a value of 9<sup>3.11</sup> 928, whereas the combined complexity of Modules B & C are 75 +  149. 

So using Glass's complexity measure the combined complexity of Modules B & C is ~1/4 of the complexity of the original Monolith. _However_ we must also include the complexity introduced by the new composite (B+C). There is only 1 Monolith, however two MicroServices are requiredto create the functionally equivalent composite. Hence the new 'composition layer' is ~8 times more complex than the opaque Monolith. Complexities at each structural layer are additive, so the total complexity of the composite structure is 75 + 149 + 9 = 232: still substantially less than the Monolith (928).

To complete the logical exercise, lets what happens if we ignore any Cohesion concerns and break the Monolith into 9 isolated Modules with one function each. Now complexity of each Module is 1. However the complexity of the composite assembly is now 9<sup>3.11</sup> 928. Now rather than being hidden within the Monolith (a significant Developer headache), complexity is exposed at higher layers of the struture hierarchy and a potential Operational risk (i.e. the Microservices ‘Orchestration’ problem) - unless automatically managed by the runtime Platform.

![Modularity and complexity](img/ComplexityModularity2.png)

To conclude, even after accounting for the new layers of structural Complexity created by Modularity, the reduction of System Complexity - through appropriate use of Modularity - is profound. To realise these significant benefits appropriate Developer tooling is required to manage the additional layers of structural abstraction, and the target runtime platform must automatically Orchestrate & Assembly such highly Modular systems while presenting Operations will a single simple entity to Manage: i.e. _shielded from Operations_ from the structural details of the composite system. 

## Complexity & TCO  

The Total Cost of Ownership is simple a financial measure of how well Systems are designed to cope with ongoing Complexity & Change!


The longer the planned life-time of the System, more Agile the Business requirements, the more volatile the runtime environment the more costly Complexity becomes and more Critically modularity becomes. Furthermore, the higher the degree of Business Agility, the more adaptive the System requirements, the more volatile the Environment, the more Loose-Coupling needs to be driven down through the Structural Hierarchy. Again we see that runtime Hierarchy, Cohesion and Loose-Coupling are related concerns.

For businesses that require Agility and / or for System that require longevity and / or must operate in volatile environmenrts, properly implemented Modularity offers transformation costs savings. To achieve this potential, applications must be design to be optimally Modular, leveraging Open Industry Standards for Modularity, the runtime environment must _fully shield_ Operations from the structural details of these composite / highly modular Applications, and ofcourse the runtime Environment itself must be Operationally simple to maintain and evolve over time.


