---
title: Persistence with JPA 
layout: toc-guide-page
lprev: 030-tutorial_microservice.html 
lnext: 015-Prerequisite.html
summary: Demonstrates how to upgrade an Application and the use JPA & Hibernate (< 10 minutes).
author: enRoute@paremus.com
sponsor: OSGi™ Alliance 
---

The previous Microservices example which uses `jdbc` provides the start-point for this tutorial.

## Creating the JPA dao-impl
 
In the `microservice` project root directory, create the `jpa` project.

      mvn -s ../settings.xml archetype:generate -DarchetypeGroupId=org.osgi.enroute.archetype -DarchetypeArtifactId=ds-component -DarchetypeVersion=7.0.0-SNAPSHOT
{: .shell }

input the following values:

    Define value for property 'groupId': org.osgi.enroute.examples.microservice
    Define value for property 'artifactId': dao-impl-jpa
    Define value for property 'version' 1.0-SNAPSHOT: : 0.0.1-SNAPSHOT
    Define value for property 'package' org.osgi.enroute.examples.microservice: : 
    Confirm properties configuration:
    groupId: org.osgi.enroute.examples.microservice
    artifactId: dao-impl-jpa
    version: 0.0.1-SNAPSHOT
    package: org.osgi.enroute.examples.microservice
    Y: : 
{: .shell } 


Add the following file `examples/microservice/dao-impl-jpa/src/main/java/org/osgi/enroute/examples/microservice/dao/impl/jpa/AddressDaoImpl.java`
<p>
  <a class="btn btn-primary" data-toggle="collapse" href="#AddressDaoImpl" aria-expanded="false" aria-controls="AddressDaoImpl">
    AddressDaoImpl.java 
  </a>
</p>
<div class="collapse" id="AddressDaoImpl">
  <div class="card card-block">
{% highlight java tabsize=4 %}
{% remote_file_content https://raw.githubusercontent.com/osgi/osgi.enroute/67b725d97736b72ee2b7e1c0ff1f69455d84c91c/examples/microservice/dao-impl-jpa/src/main/java/org/osgi/enroute/examples/microservice/dao/impl/jpa/AddressDaoImpl.java %}
{% endhighlight %}

  </div>
</div>

Add the following file `examples/microservice/dao-impl-jpa/src/main/java/org/osgi/enroute/examples/microservice/dao/impl/jpa/PersonDaoImpl.java`
<p>
  <a class="btn btn-primary" data-toggle="collapse" href="#PersonDaoImpl" aria-expanded="false" aria-controls="PersonDaoImpl">
    PersonDaoImpl.java 
  </a>
</p>
<div class="collapse" id="PersonDaoImpl">
  <div class="card card-block">
{% highlight java tabsize=4 %}
{% remote_file_content https://raw.githubusercontent.com/osgi/osgi.enroute/67b725d97736b72ee2b7e1c0ff1f69455d84c91c/examples/microservice/dao-impl-jpa/src/main/java/org/osgi/enroute/examples/microservice/dao/impl/jpa/PersonDaoImpl.java %}
{% endhighlight %}

  </div>
</div>

### Creating the JPA Entity

Create the directory `examples/microservice/dao-impl-jpa/src/main/java/org/osgi/enroute/examples/microservice/dao/impl/jpa/entities`

Add the following file `examples/microservice/dao-impl-jpa/src/main/java/org/osgi/enroute/examples/microservice/dao/impl/jpa/entities/AddressEntity.java`
<p>
  <a class="btn btn-primary" data-toggle="collapse" href="#AddressEntity" aria-expanded="false" aria-controls="AddressEntity">
    AddressEntity.java
  </a>
</p>
<div class="collapse" id="AddressEntity">
  <div class="card card-block">
{% highlight java tabsize=4 %}
{% remote_file_content https://raw.githubusercontent.com/osgi/osgi.enroute/67b725d97736b72ee2b7e1c0ff1f69455d84c91c/examples/microservice/dao-impl-jpa/src/main/java/org/osgi/enroute/examples/microservice/dao/impl/jpa/entities/AddressEntity.java %}
{% endhighlight %}

  </div>
</div>


Add the following file `examples/microservice/dao-impl-jpa/src/main/java/org/osgi/enroute/examples/microservice/dao/impl/jpa/entities/PersonEntity.java`
<p>
  <a class="btn btn-primary" data-toggle="collapse" href="#PersonEntity" aria-expanded="false" aria-controls="PersonEntity">
    PersonEntity.java
  </a>
</p>
<div class="collapse" id="PersonEntity">
  <div class="card card-block">
{% highlight java tabsize=4 %}
{% remote_file_content https://raw.githubusercontent.com/osgi/osgi.enroute/67b725d97736b72ee2b7e1c0ff1f69455d84c91c/examples/microservice/dao-impl-jpa/src/main/java/org/osgi/enroute/examples/microservice/dao/impl/jpa/entities/PersonEntity.java %}
{% endhighlight %}

  </div>
</div>


Add the following file `examples/microservice/dao-impl-jpa/src/main/java/org/osgi/enroute/examples/microservice/dao/impl/jpa/entities/persistence.xml`
<p>
  <a class="btn btn-primary" data-toggle="collapse" href="#persistence" aria-expanded="false" aria-controls="persistence">
    persistence.xml 
  </a>
</p>
<div class="collapse" id="persistence">
  <div class="card card-block">
{% highlight xml tabsize=4 %}
{% remote_file_content  https://raw.githubusercontent.com/osgi/osgi.enroute/R7/examples/microservice/dao-impl-jpa/src/main/resources/META-INF/persistence.xml%}
{% endhighlight %}

  </div>
</div>


Add the following file to `examples/microservice/dao-impl-jpa/src/main/java/org/osgi/enroute/examples/microservice/dao/impl/jpa/entities/tables.sql`
<p>
  <a class="btn btn-primary" data-toggle="collapse" href="#tables" aria-expanded="false" aria-controls="tables">
   tables.sql 
  </a>
</p>
<div class="collapse" id="tables">
  <div class="card card-block">
{% highlight sql tabsize=4 %}
{% remote_file_content https://raw.githubusercontent.com/osgi/osgi.enroute/R7/examples/microservice/dao-impl-jpa/src/main/resources/META-INF/tables.sql%}
{% endhighlight %}

  </div>
</div>


Add the following file `examples/microservice/dao-impl-jpa/src/main/java/org/osgi/enroute/examples/microservice/dao/impl/jpa/entities/package-info.java`
<p>
  <a class="btn btn-primary" data-toggle="collapse" href="#package-info" aria-expanded="false" aria-controls="package-info">
    package-info.java
  </a>
</p>
<div class="collapse" id="package-info">
  <div class="card card-block">
{% highlight java tabsize=4 %}
{% remote_file_content https://raw.githubusercontent.com/osgi/osgi.enroute/67b725d97736b72ee2b7e1c0ff1f69455d84c91c/examples/microservice/dao-impl-jpa/src/main/java/org/osgi/enroute/examples/microservice/dao/impl/jpa/entities/package-info.java %}
{% endhighlight %}

  </div>
</div>


Add the following file `examples/microservice/dao-impl-jpa/bnd.bnd`
<p>
  <a class="btn btn-primary" data-toggle="collapse" href="#bnd" aria-expanded="false" aria-controls="bnd">
    bnd.bnd
  </a>
</p>
<div class="collapse" id="bnd">
  <div class="card card-block">
{% highlight shell tabsize=4 %}
{% remote_file_content https://raw.githubusercontent.com/osgi/osgi.enroute/67b725d97736b72ee2b7e1c0ff1f69455d84c91c/examples/microservice/dao-impl-jpa/bnd.bnd %}
{% endhighlight %}

  </div>
</div>


### Dependencies

Edit `pom.xml` to add the following dependencies in the `<dependencies>` section:

{% highlight xml %}
        <dependency>
            <groupId>org.osgi.enroute</groupId>
            <artifactId>enterprise-api</artifactId>
            <type>pom</type>
        </dependency>
        <dependency>
            <groupId>org.osgi.enroute.examples.microservice</groupId>
            <artifactId>dao-api</artifactId>
            <version>0.0.1-SNAPSHOT</version>
        </dependency>
{% endhighlight %}


## The JPA Composite Application 

Create the alternative jpa application project

     $ mvn -s ../settings.xml archetype:generate -DarchetypeGroupId=org.osgi.enroute.archetype -DarchetypeArtifactId=application -DarchetypeVersion=7.0.0-SNAPSHOT
{: .shell }

    Define value for property 'groupId': org.osgi.enroute.examples.microservice
    Define value for property 'artifactId': rest-app-jpa
    Define value for property 'version' 1.0-SNAPSHOT: : 0.0.1-SNAPSHOT
    Define value for property 'package' org.osgi.enroute.examples.microservice: : 
    Define value for property 'impl-artifactId': dao-impl-jpa
    Define value for property 'impl-groupId' org.osgi.enroute.examples.microservice: : 
    Define value for property 'impl-version' 0.0.1-SNAPSHOT: : 
    Confirm properties configuration:
    groupId: org.osgi.enroute.examples.microservice
    artifactId: rest-app-jpa
    version: 0.0.1-SNAPSHOT
    package: org.osgi.enroute.examples.microservice
    impl-artifactId: dao-impl-jpa
    impl-groupId: org.osgi.enroute.examples.microservice
    impl-version: 0.0.1-SNAPSHOT
    Y: : 
{: .shell }


### Define Runtime Entity

Add the following sections to `rest-app-jpa.bndrun`

    -resolve.effective: active

    -runpath: org.jboss.spec.javax.transaction.jboss-transaction-api_1.2_spec;version=1.0.1.Final

    -runsystempackages: \
      javax.transaction;version=1.2.0,\
      javax.transaction.xa;version=1.2.0,\
      javax.xml.stream;version=1.0.0,\
      javax.xml.stream.events;version=1.0.0,\
      javax.xml.stream.util;version=1.0.0 

Edit the following section in `res-app-jpa.bndrun`:

    -runrequires: \
        osgi.identity;filter:='(osgi.identity=org.osgi.enroute.examples.microservice.rest-service)',\
        osgi.identity;filter:='(osgi.identity=org.apache.johnzon.core)',\
        osgi.identity;filter:='(osgi.identity=org.h2)',\
        bnd.identity;version='0.0.1.201801031655';id='org.osgi.enroute.examples.microservice.rest-app-jpa'


### Dependencies

Edit `pom.xml` to add the following dependencies in the `<dependencies>` section:

{% highlight xml %}
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
        <dependency>
            <groupId>org.hibernate</groupId>
            <artifactId>hibernate-osgi</artifactId>
            <version>5.2.12.Final</version>
        </dependency>
        <dependency>
            <groupId>org.apache.servicemix.bundles</groupId>
            <artifactId>org.apache.servicemix.bundles.antlr</artifactId>
            <version>2.7.7_5</version>
        </dependency>
        <dependency>
            <groupId>org.apache.servicemix.bundles</groupId>
            <artifactId>org.apache.servicemix.bundles.dom4j</artifactId>
            <version>1.6.1_5</version>
        </dependency>
{% endhighlight %}


### Runtime Configuration

Add the following configuration file `examples/microservice/rest-app-jpa/sec/main/resources/OSGI-INF/congiguratior/configuration.json`:
<p>
  <a class="btn btn-primary" data-toggle="collapse" href="#configuration" aria-expanded="false" aria-controls="configuration">
   configuration.json 
  </a>
</p>
<div class="collapse" id="configuration">
  <div class="card card-block">
{% highlight json tabsize=4 %}
{% remote_file_content https://raw.githubusercontent.com/osgi/osgi.enroute/67b725d97736b72ee2b7e1c0ff1f69455d84c91c/examples/microservice/rest-app-jpa/src/main/resources/OSGI-INF/configurator/configuration.json%}
{% endhighlight %}

  </div>
</div>

