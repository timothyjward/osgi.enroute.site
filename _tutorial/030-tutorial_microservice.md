---
title: Microservices
layout: toc-guide-page
lprev: 022-tutorial_osgi_runtime.html 
lnext: 015-Prerequisite.html
summary: Creating a REST based 3-tier Microservice with JDBC persistence (< 10 minutes).
author: enRoute@paremus.com
sponsor: OSGi™ Alliance 
---

## Introduction

This Tutorial walks through the creation of REST microservice comprised of the following structural elements:
* An API module
* A DAO implemetation module
* A Rest Service module 
* An application module

As with the Quick Start Tutorial, the following is Maven and command-line based; the reader may follow this verbatim or use their favorite Java/IDE. 

We start by creating the required project skeleton. 

if you change the example `groupId`, `artifactId` values to alternates, you'll also need to update the `packageinfo` and `import` statements in the files used in this tutorial.
{: .note }

In the directory with the `settings.xml` configuration (i.e. the directory above the [Quick Start](tutorial_qs) project home), create the **microservice** project as follows:

{% highlight shell-session %}
$ mvn -s settings.xml archetype:generate -DarchetypeGroupId=org.osgi.enroute.archetype -DarchetypeArtifactId=project-bare -DarchetypeVersion=7.0.0-SNAPSHOT
{% endhighlight %}

input the following values:

    Define value for property 'groupId': org.osgi.enroute.examples
    Define value for property 'artifactId': microservice
    Define value for property 'version' 1.0-SNAPSHOT: : 0.0.1-SNAPSHOT
    Define value for property 'package' org.osgi.enroute.examples: :
    Confirm properties configuration:
    groupId: org.osgi.enroute.examples
    artifactId: microservice
    version: 0.0.1-SNAPSHOT
    package: org.osgi.enroute.examples
    Y: :

We'll now created the required modules. 

## The microservice DAO API 

Now change directory into the newly created `microservice` project directory

To create the `api` module

{% highlight shell-session %}
$ mvn -s ../settings.xml archetype:generate -DarchetypeGroupId=org.osgi.enroute.archetype -DarchetypeArtifactId=api -DarchetypeVersion=7.0.0-SNAPSHOT
{% endhighlight %}

input the following values:

    Define value for property 'groupId': org.osgi.enroute.examples.microservice
    Define value for property 'artifactId': dao-api
    Define value for property 'version' 1.0-SNAPSHOT: : 0.0.1-SNAPSHOT
    Define value for property 'package' org.osgi.enroute.examples.microservice: :
    Confirm properties configuration:
    groupId: org.osgi.enroute.examples.microservice
    artifactId: dao-api
    version: 0.0.1-SNAPSHOT
    package: org.osgi.enroute.examples.microservice
    Y: :

* Delete the template files created in `dao-api/src/main/java/org/osgi/enroute/examples/microservice` as these are not required.
* Create the directory `dao-api/src/main/java/org/osgi/enroute/examples/microservice/dao` into which we create the following three files:

`dao-api/src/main/java/org/osgi/enroute/examples/microservice/dao/package-info.java`
<p>
  <a class="btn btn-primary" data-toggle="collapse" href="#package-info-dao" aria-expanded="false" aria-controls="package-info-dao">
    package-info.java 
  </a>
</p>
<div class="collapse" id="package-info-dao">
  <div class="card card-block">
{% highlight java tabsize=4 %}
{% remote_file_content https://raw.githubusercontent.com/timothyjward/osgi.enroute/R7/examples/microservice/dao-api/src/main/java/org/osgi/enroute/examples/microservice/dao/package-info.java %}
{% endhighlight %}

  </div>
</div>

`dao-api/src/main/java/org/osgi/enroute/examples/microservice/dao/PersonDao.java`
<p>
  <a class="btn btn-primary" data-toggle="collapse" href="#PersonDao" aria-expanded="false" aria-controls="PersonDao">
    PersonDAO.java 
  </a>
</p>
<div class="collapse" id="PersonDao">
  <div class="card card-block">

{% highlight java %}
{% remote_file_content https://raw.githubusercontent.com/timothyjward/osgi.enroute/R7/examples/microservice/dao-api/src/main/java/org/osgi/enroute/examples/microservice/dao/PersonDao.java %}
{% endhighlight %}
</div>
</div>

`dao-api/src/main/java/org/osgi/enroute/examples/microservice/dao/AddressDao.java`
<p>
  <a class="btn btn-primary" data-toggle="collapse" href="#AddressDao" aria-expanded="false" aria-controls="AddressDao">
    AddressDAO.java 
  </a>
</p>
<div class="collapse" id="AddressDao">
  <div class="card card-block">

{% highlight java %}
{% remote_file_content https://raw.githubusercontent.com/timothyjward/osgi.enroute/R7/examples/microservice/dao-api/src/main/java/org/osgi/enroute/examples/microservice/dao/AddressDao.java %}
{% endhighlight %}
</div>
</div>

Now create a `dao-api/src/main/java/org/osgi/enroute/examples/microservice/dao/dto` directory to which we add the following three files

`dao-api/src/main/java/org/osgi/enroute/examples/microservice/dao/dto/package-info.java`
<p>
  <a class="btn btn-primary" data-toggle="collapse" href="#package-info-dto" aria-expanded="false" aria-controls="package-info-dto">
    package-info.java
  </a>
</p>
<div class="collapse" id="package-info-dto">
  <div class="card card-block">

{% highlight java %}
{% remote_file_content https://raw.githubusercontent.com/timothyjward/osgi.enroute/R7/examples/microservice/dao-api/src/main/java/org/osgi/enroute/examples/microservice/dao/dto/package-info.java %}
{% endhighlight %}
</div>
</div>


`dao-api/src/main/java/org/osgi/enroute/examples/microservice/dao/dto/PersonDTO.java`
<p>
  <a class="btn btn-primary" data-toggle="collapse" href="#PersonDTO" aria-expanded="false" aria-controls="PersonDTO">
    PersonDTO.java 
  </a>
</p>
<div class="collapse" id="PersonDTO">
  <div class="card card-block">

{% highlight java %}
{% remote_file_content https://raw.githubusercontent.com/timothyjward/osgi.enroute/R7/examples/microservice/dao-api/src/main/java/org/osgi/enroute/examples/microservice/dao/dto/PersonDTO.java %}
{% endhighlight %}
</div>
</div>

`dao-api/src/main/java/org/osgi/enroute/examples/microservice/dao/dto/AddressDTO.java`
<p>
  <a class="btn btn-primary" data-toggle="collapse" href="#AddressDTO" aria-expanded="false" aria-controls="AddressDTO">
   AddressDTO.java 
  </a>
</p>
<div class="collapse" id="AddressDTO">
  <div class="card card-block">

{% highlight java %}
{% remote_file_content https://raw.githubusercontent.com/timothyjward/osgi.enroute/R7/examples/microservice/dao-api/src/main/java/org/osgi/enroute/examples/microservice/dao/dto/AddressDTO.java %}
{% endhighlight %} 
</div>
</div>

## The microservice DAO Impl

Check that you are in the `microservice` project directory, then ...

To create the `impl` module

{% highlight shell-session %}
$ mvn -s ../settings.xml archetype:generate -DarchetypeGroupId=org.osgi.enroute.archetype -DarchetypeArtifactId=ds-component -DarchetypeVersion=7.0.0-SNAPSHOT
{% endhighlight %}

input the following values:

    Define value for property 'groupId': org.osgi.enroute.examples.microservice
    Define value for property 'artifactId': dao-impl
    Define value for property 'version' 1.0-SNAPSHOT: : 0.0.1-SNAPSHOT
    Define value for property 'package' org.osgi.enroute.examples.microservice: :
    Confirm properties configuration:
    groupId: org.osgi.enroute.examples.microservice
    artifactId: dao-impl
    version: 0.0.1-SNAPSHOT
    package: org.osgi.enroute.examples.microservice
    Y: :

Now add the following module dependencies to the `<dependencies>` section in `dao-impl/pom.xml`

{% highlight xml %}
<dependency>
    <groupId>org.osgi.enroute.examples.microservice</groupId>
    <artifactId>dao-api</artifactId>
    <version>0.0.1-SNAPSHOT</version>
</dependency>
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-api</artifactId>
    <version>1.7.25</version>
</dependency>
{% endhighlight %}

* Delete the template files in `dao-impl/src/main/java/org/osgi/enroute/examples/microservice` as these are not required. 
* Now create the directory `dao-impl/src/main/java/org/osgi/enroute/examples/microservice/dao/impl` into which we create the following four files:

`dao-impl/src/main/java/org/osgi/enroute/examples/microservice/dao/impl/PersonTable.java`
<p>
  <a class="btn btn-primary" data-toggle="collapse" href="#PersonTable" aria-expanded="false" aria-controls="PersonTable">
    PersonTable.java 
  </a>
</p>
<div class="collapse" id="PersonTable">
  <div class="card card-block">


{% highlight java %}
{% remote_file_content https://raw.githubusercontent.com/timothyjward/osgi.enroute/R7/examples/microservice/dao-impl/src/main/java/org/osgi/enroute/examples/microservice/dao/impl/PersonTable.java %}
{% endhighlight %}
</div>
</div>


`dao-impl/src/main/java/org/osgi/enroute/examples/microservice/dao/impl/PersonDaoImpl.java`
<p>
  <a class="btn btn-primary" data-toggle="collapse" href="#PersonDaoImpl" aria-expanded="false" aria-controls="PersonDaoImpl">
    PersonDaoImpl.java 
  </a>
</p>
<div class="collapse" id="PersonDaoImpl">
  <div class="card card-block">


{% highlight java %}
{% remote_file_content https://raw.githubusercontent.com/timothyjward/osgi.enroute/R7/examples/microservice/dao-impl/src/main/java/org/osgi/enroute/examples/microservice/dao/impl/PersonDaoImpl.java %}
{% endhighlight %}
</div>
</div>


`dao-impl/src/main/java/org/osgi/enroute/examples/microservice/dao/impl/AddressTable.java`
<p>
  <a class="btn btn-primary" data-toggle="collapse" href="#AddressTable" aria-expanded="false" aria-controls="AddressTable">
    AddressTable.java 
  </a>
</p>
<div class="collapse" id="AddressTable">
  <div class="card card-block">

{% highlight java %}
{% remote_file_content https://raw.githubusercontent.com/timothyjward/osgi.enroute/R7/examples/microservice/dao-impl/src/main/java/org/osgi/enroute/examples/microservice/dao/impl/AddressTable.java %}
{% endhighlight %}
</div>
</div>


`dao-impl/src/main/java/org/osgi/enroute/examples/microservice/dao/impl/AddressDaoImpl.java`
<p>
  <a class="btn btn-primary" data-toggle="collapse" href="#AddressDaoImpl" aria-expanded="false" aria-controls="AddressDaoImpl">
    AddressDaoImpl.java 
  </a>
</p>
<div class="collapse" id="AddressDaoImpl">
  <div class="card card-block">


{% highlight java %}
{% remote_file_content  https://raw.githubusercontent.com/timothyjward/osgi.enroute/R7/examples/microservice/dao-impl/src/main/java/org/osgi/enroute/examples/microservice/dao/impl/AddressDaoImpl.java%}
{% endhighlight %}
</div>
</div>

## The REST Service

Check that you are in the `microservice` project directory, then ...

To create the `rest-component` module

{% highlight shell-session %}
$ mvn -s ../settings.xml archetype:generate -DarchetypeGroupId=org.osgi.enroute.archetype -DarchetypeArtifactId=rest-component -DarchetypeVersion=7.0.0-SNAPSHOT
{% endhighlight %}

input the following values:

    Define value for property 'groupId': org.osgi.enroute.examples.microservice
    Define value for property 'artifactId': rest-service
    Define value for property 'version' 1.0-SNAPSHOT: : 0.0.1-SNAPSHOT
    Define value for property 'package' org.osgi.enroute.examples.microservice: :
    Confirm properties configuration:
    groupId: org.osgi.enroute.examples.microservice
    artifactId: rest-service
    version: 0.0.1-SNAPSHOT
    package: org.osgi.enroute.examples.microservice
    Y: :

Now add the following module dependencies to the `<dependencies>` section in `rest-service/pom.xml`

{% highlight xml %}
<dependency>
    <groupId>org.apache.servicemix.specs</groupId>
    <artifactId>org.apache.servicemix.specs.json-api-1.1</artifactId>
    <version>2.9.0</version>
</dependency>
<dependency>
    <groupId>org.osgi.enroute.examples.microservice</groupId>
    <artifactId>dao-api</artifactId>
    <version>0.0.1-SNAPSHOT</version>
</dependency>
<dependency>
    <groupId>org.apache.johnzon</groupId>
    <artifactId>johnzon-core</artifactId>
    <version>1.1.0</version>
</dependency>
{% endhighlight %}

* Delete the itemplate files in `rest-service/src/main/java/org/osgi/enroute/examples/microservice` as these are not required. 
* Now create the directory `rest-service/src/main/java/org/osgi/enroute/examples/microservice/rest` into which we create the following two files: 

`rest-service/src/main/java/org/osgi/enroute/examples/microservice/rest/RestComponentImpl.java`
<p>
  <a class="btn btn-primary" data-toggle="collapse" href="#RestComponentImpl" aria-expanded="false" aria-controls="RestComponentImpl">
    RestComponentImpl.java 
  </a>
</p>
<div class="collapse" id="RestComponentImpl">
  <div class="card card-block">


{% highlight java %}
{% remote_file_content https://raw.githubusercontent.com/timothyjward/osgi.enroute/R7/examples/microservice/rest-service/src/main/java/org/osgi/enroute/examples/microservice/rest/RestComponentImpl.java %}
{% endhighlight %}
</div>
</div>


`rest-service/src/main/java/org/osgi/enroute/examples/microservice/rest/JsonpConvertingPlugin.java`
<p>
  <a class="btn btn-primary" data-toggle="collapse" href="#JsonpConvertingPlugin" aria-expanded="false" aria-controls="JsonpConvertingPlugin">
   JsonpConvertingPlugin.java 
  </a>
</p>
<div class="collapse" id="JsonpConvertingPlugin">
  <div class="card card-block">


{% highlight java %}
{% remote_file_content https://raw.githubusercontent.com/timothyjward/osgi.enroute/R7/examples/microservice/rest-service/src/main/java/org/osgi/enroute/examples/microservice/rest/JsonpConvertingPlugin.java %}
{% endhighlight %}
</div>
</div>


We now create the directory `rest-service/src/main/resources/static/main/html` in which we create the following file:

`rest-service/src/main/resources/static/main/html/person.html`
<p>
  <a class="btn btn-primary" data-toggle="collapse" href="#person" aria-expanded="false" aria-controls="person">
    person.html
  </a>
</p>
<div class="collapse" id="person">
  <div class="card card-block">


{% highlight html %}
{% remote_file_content https://raw.githubusercontent.com/timothyjward/osgi.enroute/R7/examples/microservice/rest-service/src/main/resources/static/main/html/person.html %}
{% endhighlight %}  
</div>
</div>

And we create the `rest-service/src/main/resources/static/css` directory for the `style.css` file

`rest-service/src/main/resources/static/css/style.css`
<p>
  <a class="btn btn-primary" data-toggle="collapse" href="#style" aria-expanded="false" aria-controls="style">
   style.css 
  </a>
</p>
<div class="collapse" id="style">
  <div class="card card-block">


{% highlight css %}
{% remote_file_content https://raw.githubusercontent.com/timothyjward/osgi.enroute/R7/examples/microservice/rest-service/src/main/resources/static/css/style.css %}
{% endhighlight %}
</div>
</div>

In directory `rest-service/src/main/resources/static` add the following `index.html`

`rest-service/src/main/resources/static/index.html`
<p>
  <a class="btn btn-primary" data-toggle="collapse" href="#index" aria-expanded="false" aria-controls="index">
    index.html 
  </a>
</p>
<div class="collapse" id="index">
  <div class="card card-block">


{% highlight html %}
{% remote_file_content https://raw.githubusercontent.com/timothyjward/osgi.enroute/R7/examples/microservice/rest-service/src/main/resources/static/index.html %}
{% endhighlight %}
</div>
</div>

Finally create the directory `rest-service/src/main/resources/static/main/img` into which save the following icon with the name `enroute-logo-64.png`

![enRoute logo](img/enroute-logo-64.png) 


## The Composite Application 

Check that you are in the `microservice` project directory, then ...

To create the `application` module

{% highlight shell-session %}
$ mvn -s ../settings.xml archetype:generate -DarchetypeGroupId=org.osgi.enroute.archetype -DarchetypeArtifactId=application -DarchetypeVersion=7.0.0-SNAPSHOT
{% endhighlight %}

input the following values:

    Define value for property 'groupId': org.osgi.enroute.examples.microservice
    Define value for property 'artifactId': rest-app
    Define value for property 'version' 1.0-SNAPSHOT: : 0.0.1-SNAPSHOT
    Define value for property 'package' org.osgi.enroute.examples.microservice: :
    Define value for property 'impl-artifactId': dao-impl
    Define value for property 'impl-groupId' org.osgi.enroute.examples.microservice: :
    Define value for property 'impl-version' 0.0.1-SNAPSHOT: :
    Confirm properties configuration:
    groupId: org.osgi.enroute.examples.microservice
    artifactId: rest-app
    version: 0.0.1-SNAPSHOT
    package: org.osgi.enroute.examples.microservice
    impl-artifactId: dao-impl
    impl-groupId: org.osgi.enroute.examples.microservice
    impl-version: 0.0.1-SNAPSHOT
    Y: :

Add the following dependencies inside `<dependencies>` section of the file `rest-app/pom.xml`

{% highlight xml %}
<dependency>
    <groupId>org.osgi.enroute</groupId>
    <artifactId>osgi-api</artifactId>
    <type>pom</type>
</dependency>
<dependency>
    <groupId>org.osgi.enroute.examples.microservice</groupId>
    <artifactId>rest-service</artifactId>
    <version>0.0.1-SNAPSHOT</version>
</dependency>
<dependency>
    <groupId>org.apache.johnzon</groupId>
    <artifactId>johnzon-core</artifactId>
    <version>1.1.0</version>
</dependency>
<dependency>
    <groupId>com.h2database</groupId>
    <artifactId>h2</artifactId>
    <version>1.4.196</version>
    <scope>runtime</scope>
</dependency>
{% endhighlight %}

Also add the following plugin inside `<plugins>` section in the file `rest-app/pom.xml`

{% highlight xml %}
<plugin>
    <groupId>biz.aQute.bnd</groupId>
    <artifactId>bnd-maven-plugin</artifactId>
</plugin>
{% endhighlight %}

Create the directory `rest-app/src/main/java/config` in which 

`rest-app/src/main/java/config/package-info.java`
<p>
  <a class="btn btn-primary" data-toggle="collapse" href="#package-info-config" aria-expanded="false" aria-controls="package-info-config">
    package-info.java
  </a>
</p>
<div class="collapse" id="package-info-config">
  <div class="card card-block">

{% highlight java %}
{% remote_file_content https://raw.githubusercontent.com/timothyjward/osgi.enroute/R7/examples/microservice/rest-app/src/main/java/config/package-info.java %}
{% endhighlight %}
</div>
</div>


Create the directory `rest-app/src/main/resources/OSGI-INF/configurator` and then the following file 

`rest-app/src/main/resources/OSGI-INF/configurator/configuration.json`
<p>
  <a class="btn btn-primary" data-toggle="collapse" href="#configuration" aria-expanded="false" aria-controls="configuration">
    configuration.json
  </a>
</p>
<div class="collapse" id="configuration">
  <div class="card card-block">


{% highlight json %}
{% remote_file_content https://raw.githubusercontent.com/timothyjward/osgi.enroute/R7/examples/microservice/rest-app/src/main/resources/OSGI-INF/configurator/configuration.json %}
{% endhighlight %}
</div>
</div>

Copy the contents below over `rest-app/rest-app.bndrun`

`rest-app/rest-app.bndrun`
{% highlight shell-session %}
index: target/index.xml
 
-standalone: ${index}
 
-resolve.effective: active
 
-runrequires: \
    osgi.identity;filter:='(osgi.identity=org.osgi.enroute.examples.microservice.rest-service)',\
    osgi.identity;filter:='(osgi.identity=org.apache.johnzon.core)',\
    osgi.identity;filter:='(osgi.identity=org.h2)',\
    bnd.identity;version='0.0.1.201801031655';id='org.osgi.enroute.examples.microservice.rest-app'
-runfw: org.apache.felix.framework
-runee: JavaSE-1.8
{% endhighlight %}

## Build & Run

Build the modules and install in local maven repository from the top level project directory

    mvn install

We now Generate OSGi™ index with the project dependencies from the top level project directory

    mvn bnd-resolver:resolve

And generate the runnable jar from the top level project directory

    mvn package

To run the resultant OSGi based REST Microservice change back to the top level project directory and run

    java -jar rest-app/target/rest-app.jar

The REST service can be seen by pointing a browser to [http://localhost:8080/microservice/index.html](http://localhost:8080/microservice/index.html)

![MicroService demo](img/MicroService.png)

Stop the application using Ctrl+C in the console.


