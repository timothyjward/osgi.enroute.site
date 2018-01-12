---
title: Services 
layout: services
lprev: org.osgi.util.tracker.html 
lnext: 215-sos.html
---

This section introduces service oriented design, provides an overview of OSGi™ Declarative Services and then provides information on some of the most important OSGi™ services and usage patterns. 

<div>
<table>

{% for service in site.services %}<tr><td><a href="{{service.url}}">{{service.title}}</a></td><td>{{service.summary}}</td></tr>
{% endfor %}

</table>
</div>
