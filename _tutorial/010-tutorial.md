---
title: Tutorials 
layout: tutorial
lprev: 020-tutorial_qs.html 
lnext: 015-Prerequisite.html 
---

This sequence of tutorials start with a simple Microservice and go on to create increasingly sophisticated distributed applications. The time required to complete each tutorial is indicated in each Tutorial's description.

These tutorials focus on the correct use of OSGi™ specifications; a basic knowledge of Java, Git, Eclipse & Maven are assumed.

<div>
<table>

{% for tutorial in site.tutorial %}<tr><td><a href="{{tutorial.url}}">{{tutorial.title}}</a></td><td>{{tutorial.summary}}</td></tr>
{% endfor %}

</table>
</div>

