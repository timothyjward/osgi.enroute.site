---
title: OSGi enRoute Archetypes 
layout: toc-guide-page
lprev: 015-Prerequisite.html 
lnext: 020-tutorial_qs.html
summary: Explanation of the Maven Archetypes used by enRoute  
---


An archetype is a type of Maven project template which is used to generate your build so that you can get up and running quickly. It’s absolutely possible to use enRoute without using these archetypes, but they are recommended as a simple way to configure and manage your OSGi build.

All of the enRoute archetypes are found using the org.osgi.enroute.archetype group id. They all make use of build plugins from the bnd project, and they all make use of the OSGi APIs and Reference Implementations as defined by the OSGi enRoute indexes.

There are two main kinds of OSGi enRoute Archetypes:

* Project Archetypes
* Module Archetypes

## Project Archetypes

A project archetype is a template that you’ll use relatively infrequently because its purpose is to set up a completely new OSGi enRoute workspace. The project archetype will create your top-level reactor POM, and define configuration templates for all of the build plugins and dependencies that you’re likely to use in your OSGi enRoute applications.

There are two enRoute project Archetypes:

* The project archetype
* The project-bare archetype

### The project Archetype

The project archetype is designed to get you up and running as fast as possible, setting up a top-level reactor POM and a simple OSGi enRoute application.

This simple application consists of a Declarative Services component project and an enRoute application project. Together these provide a simple application, just like you can see in the quickstart example.

### The project-bare Archetype
The project-bare archetype creates an absolutely minimal enRoute workspace. It doesn’t contain any modules, giving you the maximum flexibility to create your own application modules

## Module Archetypes

A module archetype defines an enRoute Maven module that will be built and output an artifact. The output of the module will depend on the type of archetype that you use:

* The api archetype
* The ds-component archetype
* The rest-component archetype
* The application archetype

### The api Archetype

The api archetype is used to build an API bundle for your enRoute application. This type of bundle is where you should define your public interfaces and DTOs. These packages should be versioned and exported.
The ds-component Archetype

### The ds-component Archetype 
The ds-component Archetype is used to create an OSGi service using Declarative Services. This provides a simple programming model for referencing services from the OSGi service registry and then publishing your implementation as a service. Declarative Services uses annotations to define components and injection sites, and these annotations are ready for use in the basic component.

### The rest-component Archetype

The rest-component archetype is similar to the ds-component  archetype, in that it defines an OSGi service using Declarative Services. This service, however, makes use of the OSGi JAX-RS whiteboard specification to transparently provide the JAX-RS resource methods as REST endpoints. This archetype is therefore an excellent way to get started when writing a REST microservice                                                   
### The application Archetype

The application archetype is different from the other enRoute modules, in that its output is not an OSGi bundle but rather a runnable application. The application archetype is designed to reference the other modules in your application, and use them to resolve an application based on your runtime requirements.

These requirements are provided in a bndrun file, which defines how the OSGi application should be launched, and what should be contained in it. This bndrun can be resolved to turn its requirements into a list of bundles to run, and then exported into a runnable jar file.

## Project Setup

To prepare for the [tutorials] past the following Maven project skeleton to a file named `settings.xml` into your project root directory.

{% highlight html %}
    <settings>
      <profiles>
        <profile>
          <id>OSGi</id>
          <activation>
            <activeByDefault>true</activeByDefault>
          </activation>
          <repositories>
            <repository>
              <id>osgi-archetype</id>
              <url>https://oss.sonatype.org/content/groups/osgi</url>
              <releases>
                <enabled>true</enabled>
                <checksumPolicy>fail</checksumPolicy>
              </releases>
              <snapshots>
                <enabled>true</enabled>
                <checksumPolicy>warn</checksumPolicy>
              </snapshots>
            </repository>
          </repositories>
        </profile>
      </profiles>
    </settings>
{% endhighlight %}


