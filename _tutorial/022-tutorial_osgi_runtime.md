---
title: OSGi Runtime & Debug  
layout: toc-guide-page
lprev: 020-tutorial_qs.html  
lnext: 030-tutorial_microservice.html 
summary: A peek behind the curtain - the OSGi™ µServices runtime!
author: enRoute@paremus.com
sponsor: OSGi™ Alliance  
---

## Summary 

In the `quickstart` tutorial we created and ran a simple OSGi™ Microservice. Before progressing to more sophisticated examples we'll first take a quick look at the OSGi™ runtime and the software DNA of your Microservices application.

To gain access to the OSGi runtime we need to create a **debug** version of `quickstart`.

## Creating the debug version of quickstart 

From the `quickstart` project root directory you previously created, change directory to `./app` edit the `pom.xml`, and changing the first occurance of `<bndrun>app.bndrun</bndrun>` 

      <plugin>
          <groupId>biz.aQute.bnd</groupId>
          <artifactId>bnd-export-maven-plugin</artifactId>
          <configuration>
              <bndruns>
                  <bndrun>app.bndrun</bndrun>
              </bndruns>
          </configuration>
      </plugin>
      <plugin>
          <groupId>biz.aQute.bnd</groupId>
          <artifactId>bnd-resolver-maven-plugin</artifactId>
          <configuration>
              <bndruns>
                  <bndrun>app.bndrun</bndrun>
                  <bndrun>debug.bndrun</bndrun>
              </bndruns>
          </configuration>
      </plugin>


to `<bndrun>debug.bndrun</bndrun>` as shown

      <plugin>
           <groupId>biz.aQute.bnd</groupId>
           <artifactId>bnd-export-maven-plugin</artifactId>
           <configuration>
               <bndruns>
                   <bndrun>debug.bndrun</bndrun>
               </bndruns>
           </configuration>
       </plugin>
       <plugin>
           <groupId>biz.aQute.bnd</groupId>
           <artifactId>bnd-resolver-maven-plugin</artifactId>
           <configuration>
               <bndruns>
                   <bndrun>app.bndrun</bndrun>
                   <bndrun>debug.bndrun</bndrun>
               </bndruns>
           </configuration>
       </plugin>


Now re-build your `quickstart` application

{% highlight shell-session %}
$ mvn install
$ mvn bnd-resolver:resolve
$ mvn package 
{% endhighlight %}

and run the new debug version.

{% highlight shell-session %}
$ java -jar app/target/debug.jar 
{% endhighlight %}



