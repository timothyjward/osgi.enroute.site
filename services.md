---
layout: toc-page
title: Services 
description: Services 
permalink: /Services/
order: 3 
---

This section introduces service oriented design, provides an overview of OSGi™ Declarative Services and then provides information on some of the most important OSGi™ services and usage patterns. 

<style>
table, td, th {
    text-align: left;
}

table {
    width: 100%;
}
        
th {
    padding: 15px;
    color: Black;
}
td {
    padding 10px;
    color: Black;
}
</style>

<div>

<table>
        <colgroup>
                <col style="width:30%">
                <col style="width:70%">
        </colgroup>
{% for f in site.services %}{%unless f.noindex%}<tr>
        <td><a href="{{f.url}}">{{f.title}}</a></td><td> {{f.summary}}</td>
</tr>
{%endunless%}{% endfor %}

</table>

