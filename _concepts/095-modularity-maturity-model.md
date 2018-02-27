---
title: Complexity, Modularity & Business
summary: Modularity - The necessary foundations for Agile and Efficient Organisations 
layout: toc-guide-page 
lprev: 090-complexity-modularity 
lnext: 100-about-osgi
author: enRoute@paremus.com
sponsor: OSGi™ Alliance
---

## Complexity & Cost

> The most import measure of Complexity is the financial cost associated with software maintenance: i.e. the cost required to change a System. 

A number of independent studies concurr that software maintenance accounts for ~80% of the total life-time cost of a software system: e.g. [Erlikh 2000](Erlikh, I. (2000) Leveraging Legacy System Dollars for E-Business. Information Technology Proceedings, May/June 2000, 17-23.) determined that 80% of software costs are concerned  with **evolution** of the software.

These costs typically breakdown into following activities in the following proportions.

* Corrective (Fault Fixing) - 17%
* Adaptive (Environmental Changes) - 18%
* Perfective (New Functionality) - 61%
* Other - 4%
 
To provide a financial context, lets apply this breakdown to some typical project costs:

* A small Enterprise software project that costs $1M to deliver will have a lifetime cost of $5M.
* An Industry 4.0 soluton that has an implementation costs of $20M, will have a lifetime cost of $100M.
* A modest Smart City solution that has an implementation cost of $50M, will have a lifetime cost of $250M, of which ...
    * $42.5M is fault fixing.
    * $45M of costs relating to changes in the Environment.
    * $152.5M ongoing functional enhancements.

Given the increasing sophistication of modern software solutions these figures almost certainly underestimate the total lifetime costs. 

Here we are ofcourse simply restating [DARPA's observation](090-complexity-modularity.html#the-problem).


## Restatment of the Problem 

For an Organisation with **K** Systems, the change related costs per period of time can be expressed as: 

<div> 
$$f(total cost) &#8733; \sum_{k=1}^{K}(Complexity_{k})&#215;(NumberOfChanges_{k})$$
</div>

where 

* $$Complexity_{k}$$ is the complexity of System {k}
* $$NumberOfChanges_{k}$$ are the number of changes that System {k} undergoes during the period of interest. 

To minimise change costs we simply need to minimise this expression.


## Traditional Approaches

If we ignoring the role of Modularity - what other approaches are available for managing Complexity? Possible approaches to bearing down on Operational costs (the proxy for Complexity) might include:
  
* **Avoid all Changes**: If there is no change, there is no cost of change. Physically impossible and contradicts the ethos of an Agile Organisation.
* **Force Conformity / Reduce Diversity**: Force all the Systems to use the same infrastructure services. Consolidate applications where possible, enforce a single architecture, a single protocol, a single language: e.g. HTTP everywhere & REST Microservices. Physically possible but again impacts Organisational Agility.
* **Hide Complexity**: Make the complexity for various layers of the runtime someone elses problem - either by wholesale Outsourcing, by using third Party Clouds (Baremetal, VM, Container, FaaS). 

Traditional IT strategies usually include one or more elements of the above. 

### Avoid Change

A **no change** strategy results in extremely fragile Operational environments. When the inevitable unplanned environmental change occurs (i.e. resource failure or Operational error), this event is much more likely to have a catastrophic effect: i.e. a Black Swan event.

Real world Organisations, even those with static business models that require no new functionality, must still respond to [security patches, fixes and accomodate environmentally forced changes](095-modularity-maturity-model#complexity--cost). 

### Consolidate Applications

To reduce Complexity, reduces the number of Applicatons by consolidation of funtionality. 

However, if we consider [Glass's law](090-complexity-modularity#qualitative-measures) we can immediately see a problem. Consider three business services each composed of 4 functions, 3 of which are the same for each System;

`System A (a,b,c,x)`, 
`System B (a,b,c,y)`, 
`System C (a,b,c,z`)
 
The Complexity estimate for these three systems would then be:

`System A (4<sup>3.11</sup>) + System B (4<sup>3.11</sup>) + System C (4<sup>3.11</sup>) =~ 224` 

Whereas the equivalent consolidate `System D (a,b,c,x,y,z)` will have a complexity measure of `6<sup>3.11</sup> = 263` 

In this example while **Operational Complexity** decreases from managing 3 applications to 1 larger application, however **Code Complexity** (the much larger issue), remains essentially unchanged. The degree of functional overlap between Systems being consolidated may be greater than 75%, or one may argue that the expotential <sup>3.11</sup> is too high for your organisation; however the argument concerning the ineffectivness of functional consolidation in reducing complexity still stands. 

Also note that Operational flexibility and resilience are reduced as the whole User population is now dependent upon the one monolithic application.  

So perhaps not the desired outcome for a multi-million dollar application consolidation program? 


### Outsourcing

Here we simply sign over our Complexity problem to a 3rd party. 

The 3rd party achieves a lower cost, not by re-engineering the applications to drive down complexity, but by preserving the status-quo and using cheaper engineeing resources to implement change requests. As changes remain challenging the 3rd Party is incentivized to minimise these, and charge a premium for exceptions not covered in the contract. 

### Virtualisation, Containers and / or 3rd Party Cloud

Here we simply move the Complexity problem from a physical platform to a virtual platform. 

Virtualisation has **no-effect** on application Complexity, and **increases** infrastructure complexity. Investment in Virtualisation or Container strategy should only be justified on infrastructure costs savings through increased resource utilisation. One must also account for potentially significant increases in infrastructure Complexity, which in-turn can lead to increased Service outages.

The same argument applies to 3rd Party Cloud providers; moving an Application to a third party Cloud environment has no effect on the Application's internal Complexity. However third Party Cloud providers - at a cost - do shield you from the infrastructure Complexity.


### Write Throw Away Code

Finally, rather than trying to maintain an existing code-base, wholesale rewrite the application using commodity developer resources each time major changes are required.  

This may be a valid strategy for a small consumer start-ups, but increasingly not so for sophisticated Telco, Financial Service, or an Industry 4.0 systems that require ongoing incremental enhancements to a complex inter-related eco-system of Services.

Ironically, the more modular the overall software ecosystem, *the simple it becomes* to rapidly rip-n-replace individual Modules.


## Current Trends 

If traditional strategies fail to address the fundamental problem of Application complexity what about current application trends? 

Do these help?

### MicroServices?

A limited modularity strategy may be pursued via adoption of REST based Microservices. 

By 'limited' we mean that large applications are broken down into a set of smaller deployable Services that communcated with each other via REST, however in most cases the internal implementation of each Microservice remains non-Modular. This is also an example of **Force Conformity / Reduce Diversity** as all applications must use HTTP/REST to communicate. 

Contrary to the consolidation [example](095-modularity-maturity-model#consolidate-applications), we now break the Systems
	`System A (a,b,c,x)`, `System B (a,b,c,y)`, `System C (a,b,c,z)` 
into the Microservices 
`µa, µb, µc, µx, µy, µz` 
which can then be re-combined into Composite Systems  
`A(µa, µb, µc, µx), B(µa, µb, µc, µy), C(µa, µb, µc, µz)` 

As disucsssed in [Qualitative Measures](090-complexity-modularity.html#qualitative-measures), while code complexity is significantly reduced we have created Orchestration complexity as a by-product: see [Complexity & Hierarchy](090-complexity-modularity.html#complexity--hierarchy). 

REST / Container centric Microservices strategies generally lack standards based mechanisms to manage this Orchestration complexity, and so the problem is exposed to Operations.
{: .p.warning}

### Twelve-Factor Applications?

Twelve-Factor methodology is a variant of the REST / Microservices trend.

The Twelve-Factor methodology encourages: 
* the use of declarative formats for setup automation - to minimize time and cost on-boarding new developers;
* the use of clean contracts to define dependencies on underlying runtime - to enabled portability; 
* to focus on minimizing divergence between development and production - to enable continuous deployment; 
* and to scale up without significant changes to tooling, architecture, or development practices.

Like Microservices, Twelve-Factor methodology is applicable to any application written in any programming language. However, just like Microservices, to achieve this language agnostic position, Twelve-Factor needs to specify a number of application design constraints which may or may not be acceptable. 

While mentioning the benefits of modularity and dependendency management Twelve-Factor methodology is vague about how this should be achieved. Both Heavyweight monoliths or highly modular Composite Systems like [our Microservices example](_tutorial/030-tutorial_microservice) may be crafted to be "Twelve-Factor" compliant.

While Twelve-Factor methodology does not address Application Complexity, like REST based Microservices it may be one of a number of useful design patterns used within the context of a more all-encompassing Modularity strategy.  


### Serverless Application Model

In a Serverless environment, developers create units of Application logic that are instantiated in third-Party Function-as-a-Service compute offerings: i.e. databases, search indexes, queues, SMS messaging, and email delivery. While very similar in concept to compute Grid offerings that were available over a decade ago; the recent _Serverless_ movement prefers to reference AWS Lambda as the inspiration. 

FaaS is an example of a **Hide Complexity** strategy, as the Developer's concern is only the Application logic, with all software infrastructure provided as third-party Services. 

State benefits of Serverless include:

* Zero administration - Deploy code without provisioning anything beforehand, or managing anything afterward. There is no concept of a fleet, an instance, or even an operating system.
* Auto-scaling - Infrastructure Service providers manage the scaling challenges. No need to fire alerts or write scripts to scale up and down.
* Avoid vendor lock-in - Different cloud providers all have different required formats and deployment methods. The Framework assembles your application into a single package that can be deployed across any Cloud provider.

Also, as Function-as-a-service is charged based on usage rather than pre-provisioned capacity, significant cost-savings (~90%) relative to an equivalent number of provisioned cloud VM's hosting the same services can be achieved. 

While the FaaS User is decoupled from the underlying runtime infrastructure provider; they are ofcourse, coupled to the Serverless framework via deployment descriptors: the runnable code needing an associated deployment descriptor (YAML/JSON) that describes all environmental dependencies and life-cycle for that framework. And while as a Developer or User you are not concerned about Orchestration or infrastructure complexity; these problems still exist for the FaaS Service Provider, and will bleed into prices charged downstream.  

If your Application problem maps to a Serverless model; if resource usage is unpredictable and bursty; and if you are happy to hand off the required software infrastructure services to a third party, then the FaaS pricing model is attractive.

However the following should also be born in mind:
* Resource costs typical only contributes ~10% to your applications life-time costs. So is a FaaS strategy actually focusing on the most important issue?
* Resource cost savings are only meaningful in a third time-shared environment; with many Users with different, un-correlated, workloads.
* Like Twelve-Factor, the Serverless paradigm only address a subset of potential application types.

Finally, while a number of open Source projects exists, currently no industry standards exist for defining dependencies, or life-cycles; hence the User is coupled to a FaaS implementation; and a significant amount of Complexity is hidden within the FaaS Service; this managed in the traditional manner.


### The Polyglot Organisation? 

Proponents of REST based Microservices, Twelve-Factor & Serverless also tend to be _Polyglot_ advocates: arguing that runtime services can be written using off-the-street developers using their favorite langangue of choice. 

However, this is inconsistent with **Force Conformity / Reduce Diversity**. From a maintenance perspective an overly indulgent Polyglot strategy is a significant Organisational problem. Consider a Microservice processing pipeline consisting of Python, Go, Rust, C++ and Haskell Service components. Maintenance of this Service either requires:
* the Organisation to maintain a diverse set of Developer skills that will be difficult and costly to maintain over extended periods;
* Or to treat all Service Components as dispoable units; to be re-written from scratch as and when new functionality is required; each time using commodity Development resources and their preferred language.

It is also worth noting that the [TIOBE Index for January 2018](https://www.tiobe.com/tiobe-index/) yet again places Java as the industries leading programming language, a position broadly held since 2003: this perhaps an indication that senior management in Organisations understand that unnecessary language diversity (unnecessary Complexity) is an **excellent mechanism** for generating explosive levels of runtime complexity (see [CISQ Technical Debt Calculation](ihttp://it-cisq.org/standards/technical-debt/)), and ofcourse future recruiting issues: i.e. _vacancy for a developer - must have 5 years experience in all of the following - Python, Go, Rust, C++ and Haskell._


### DevOps

A common response to uncontrolled Operational Complexity is DevOps. 

In the DevOps model, the Development teams are made responsible for all aspects of their Application; from code development to Production. The argument is that Developers can '*deal* ' with the Operational complexity; and in addition direct access to Production Systems - without Operational barriers - allows the developer to deploy code, and recover from the errors caused by the deployment, more rapidly. 

However, the DevOps model results in tight coupling between Developement teams and the Operational Services for which they are now responsible. This tight-coupling introduces systemic Operational Risk: talented members of a Development team are more likely to leave the company, and in a DevOps centric environment, this immediately translates to an Operational risk to the business: [The Wetware Crisis: the Dead Sea effect](http://brucefwebster.com/2008/04/11/the-wetware-crisis-the-dead-sea-effect/).


## Modularity First - The Modularity Maturity Model

Complexity is a critical business problem. Complexity cannot be ignored, cannot be consolidated away, hidden by a third Party Cloud, or an out-source contract. Directly or indirectly the costs associated with Complexity will always re-emerge. 

A Modularity first strategy is the only way that Complexity can be addressed, and high maintainable, economically sustainable software systems delivered. 

As explained in [Agility and Modularity: Two Sides of the Same Coin](https://www.osgi.org/wp-content/uploads/AgilityandModularity2014v21.pdf), senior management are familiar with the concepts of modularity and dependencies when in the context of Organisational management structures. Recognising this, the <em>Modularity Maturity Model</em> was proposed by Dr Graham Charters at the OSGi Community Event 2011, as a way of helping Management assess how far down the modularity path their organisation or project is. It is named after the [Capability Maturity Model](http://en.wikipedia.org/wiki/Capability_Maturity_Model), which allows organisations or projects to measure their improvements on a software development process.

Note that the following terminology is implementation agnostic in that it can be applied to any modularity model. The following is also intended as a guide rather than prescriptive.


### Level 1: Ad Hoc

No formal modularity exists. Dependencies are unknown. Applications have no, or limited, structure. Agile processes are likely to fail as application code bases are monolithic and highly coupled. Testing is challenging as changes propagate unchecked, causing unintentional side effects. Governance and change management are costly and acknowledged to be high-level operational risks.
 
### Level 2: Modules 

Named modules are used with explicit versioning. Dependencies are expressed in terms of module identity (including version). Maven, Ivy and RPM are examples of modularity solutions where dependencies are managed by versioned identities. Artifact repositories are used; however, their value is compromised as the artifacts are not self-describing. Agile processes are possible and do deliver some business benefit. However, the ability to realize Continuous Integration (CI) is limited by ill-defined dependencies. Governance and change management are not addressed. Testing is still failure-prone. Indeed, the testing process is now the dominant bottleneck in the agile release process. Governance and change management remain costly and continue to be high-level operational risks.

### Level 3: Modularity 

Module dependencies are now expressed via contracts (i.e. capabilities and requirements). Dependency resolution becomes the basis of software construction. Dependencies are semantically versioned, enabling the impact of change to be communicated. By enforcing strong isolation and defining dependencies in terms of capabilities and requirements, modularity enables many small development teams to efficiently work independently and in parallel. The efficiency of Scrum and Kanban management processes correspondingly increases. Sprints are now associated with one or more well-defined structural entities; i.e. the development or refactoring of OSGi bundles. Semantic versioning enables the impact of refactoring to be contained and efficiently communicated across team boundaries. Via strong modularity and isolation, parallel teams can safely sprint on different structural areas of the same application. Strong isolation and semantic versioning enable efficient/robust unit testing. Governance and change management are now demonstrably much lower operational risks.i

### Level 4: Services 

Services-based collaboration hides the construction details of services from the users of those services, so allowing clients to be decoupled from the implementations of the providers. Services lay the foundation for runtime loose coupling. The dynamic find and bind behaviors in the OSGi service model directly enable loose coupling by enabling the dynamic formation of composite applications. All local and distributed service dependencies are automatically managed. The change of perspective from code to OSGi μServices increases developer and business agility yet further: new business systems being rapidly composed from the appropriate set of pre-existing OSGi μServices.

### Level 5: Devolution

Artifact ownership is devolved to modularity-aware repositories, which encourage collaboration and enable governance. Assets may be selected on their stated capabilities. Advantages include:
 
* Greater awareness of existing modules
* Reduced duplication and increased quality
* Collaboration and empowerment
* Quality and operational control
 
As software artifacts are described in terms of a coherent set of requirements and capabilities, developers can communicate changes (breaking and non-breaking) to third parties through the use of semantic versioning. Devolution allows development teams to rapidly find third-party artifacts that meet their requirements. From a business perspective, devolution enables significant flexibility with respect to how artifacts are created, allowing distributed parties to interact in a more effective and efficient manner. Artifacts may be produced by other teams within the same organization or consumed from external third parties. The Devolution stage promotes code re-use and increases the effectiveness of offshoring/near shoring or the use of third-party, OSS or crowd-sourced software components. This, in turn, directly leads to significant and sustained reductions in operational cost.
 
### Level 6: Dynamism 

Dynamism is the culmination of the organization’s Modularity journey: this built upon Modularity, Services and Devolution.

* The Organisation is now able to automatically assembled sophisticated business Systems from modular components.
* As Application are composed from a hierarchy of self-describing components,
    * Complexity exposed to both Developers and Operations is minimised.
    * The Environment is able to dynamical assemble and maintain these self-describing Applications.
* As semantic versioning is used, the impact of change is efficiently communicated to all interested parties, including governance and change control processes.
* Individual components may be rapidly and safely deployed and /or changed in the Production environment, enabling runtimeAgilty & Adaption. 
* As the dynamic assembly process is aware of the capabilities of the hosting runtime environment, application structure and behavior may automatically adapt to location, allowing transparent deployment and optimization for public cloud or traditional private data center environments.
* Architecture neutrality is achieved. Dependent upon the business problem - REST based Microservices, 12 Factor-Apps, Asychronous Actor based AI designs, or traditional XA transactional systems are all equally valid design choices, and as requirements dictate business logic may be simply moved between these alternatives.
* The Agile foundations have been layed to now effectively leverage runtime adaptive Machine Learning & AI techniques.

![Modularity Maturity Model](img/Modularity-Maturity-Model.png)

Each Organization’s modularization migration strategy (i.e. the route to traverse these modularity levels) will be dictated by which characteristics are of most immediate business value to the Organisation. Most Organizations have moved from the initial Ad-Hoc phase to Modules. Services, in the guise of 'Microservices' is currenly popular, however few Organisations have implemented true Modularity or Devolution. Organizations that value a high degree of Agility and / or Adaptive Automation will wish to reach the Dynamism endpoint as soon as possible.

## Conclusions 

Complexity is the primary issues that affects the economic sustainability of software systems; and so a coherent strategy for Modularity - based on Open Industry standards - must be our start point. 

This Modularity strategy must define how to describe dependencies and life-cycles for all entities, at all structure layers, of each Composite System: from the fine grained artefacts created by the Component Developers, to the distributed Composite System run by Operations; enabling all layers of the runtime environment to be automatically Assembly / Orchestrated.

It is important to note that such a Modularity strategy does not exclude REST based Microservices, use of Cloud, a Twelve-Factor approach, or use of Function-as-a-Service; rather a Modularity first strategy is an enabler for **maintainable** versions of all of the these; while also ensuring that these design decisions can be changed at a later point in time in response to changing Business requirements. 

Today, OSGi™ / Java is the only modularity / language combination powerfull enough to address Complexity Crisis and so DARPA's longevity challenge. However, the OSGi Alliance are looking to generalise OSGi Dependency Management and Service Registry concepts, bringing these benefits to other popular languages.  

We'll now explore OSGi concepts and capabilities in more detail.

