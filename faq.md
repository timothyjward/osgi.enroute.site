---
layout: toc-page
title: FAQ 
description: Frequently asked Questions 
permalink: /FAQ/
order: 4
author: enRoute@paremus.com
sponsor: OSGi™ Alliance 
---
The OSGi™ Allaince has evolved a series of modularity Specifications which provide the foundations for highly maintainable, agile, and so economically sustainable software eco-systems. As part of this ongoing endevour, enRoute has been created to leverage latest OSGi™ specifications and to demonstrate that creating modular applications is actually rather simple.

Why this is fundamental to your business' future sustainability is the subject of this section. 

![Lowering the barriers](/img/book/why-enroute.png)


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
<table>
        <colgroup>
                <col style="width:30%">
                <col style="width:70%">
        </colgroup>
{% for f in site.FAQ %}{%unless f.noindex%}<tr>
        <td><a href="{{f.url}}">{{f.title}}</a></td><td> {{f.summary}}</td>
</tr>
{%endunless%}{% endfor %}

</table>


---
