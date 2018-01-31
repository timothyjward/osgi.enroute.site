---
layout: toc-page
title: Concepts 
description: Concepts 
permalink: /Concepts/
order: 4 
---
Since 1998 the OSGi™ Allaince has evolved a series of modularity Specifications which provide the foundations for highly maintainable, agile and so economically sustainable software eco-systems. As part of this ongoing endevour, enRoute has been created to address the criticism that building modular systems is too hard a task for the average developer.

![Lowering the barriers](/img/book/why-enroute.png)

Why this is so important to you & your business' future sustainability is the subject of this section. 

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
{% for f in site.concepts %}{%unless f.noindex%}<tr>
        <td><a href="{{f.url}}">{{f.title}}</a></td><td> {{f.summary}}</td>
</tr>
{%endunless%}{% endfor %}

</table>


---
