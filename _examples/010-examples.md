---
title: Examples
layout: examples 
lprev: 020-tutorial_qs.html 
lnext: 015-Prerequisite.html
---

enRoute examples illustrate the correct use of the more frequently used OSGi™ specifications. Where example code is used within an enRoute [Tutorials](/tutorial/010-tutorial.html), the approprite link this provided, along with links to the OSGi™ Service or usage patterns demonstrated by the code example.    

The examples are all in a workspace that you can clone from Github. Some examples are combined together, others have their own workspace. It is highly suggested to clone the workspace and play with the example.

<div>
<table>

{% for example in site.examples %}<tr><td><a href="{{example.url}}">{{example.title}}</a></td><td>{{summary.summary}}</td></tr>
{% endfor %}

</table>
</div>

