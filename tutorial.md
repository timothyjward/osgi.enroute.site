---
layout: toc-page
title: Tutorials 
description: Tutorials 
permalink: /Tutorials/
order: 1 
---

These Tutorials progress from the simplest Microservice on to increasingly sophisticated distributed applications. In each case the focus is on creating and running the application. A basic familiarity with Java, Git, Eclipse & Maven are assumed. 

The time required to complete each tutorial is indicated in each Tutorial's description.

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

{% for tutorial in site.tutorial %}<tr><td><a href="{{tutorial.url}}">{{tutorial.title}}</a></td><td>{{tutorial.summary}}</td></tr>
{% endfor %}

</table>
</div>


---
