---
layout: toc-page
title: Examples 
description: Examples 
permalink: /Examples/
order: 2
---
enRoute examples illustrate the correct use of the more frequently used OSGi™ specifications. Where example code is used within an enRoute Tutorial, the approprite link this provided, along with links to the OSGi™ Service or usage patterns demonstrated by the code example.    

The examples are all in a workspace that you can clone from Github. Some examples are combined together, others have their own workspace. It is highly suggested to clone the workspace and play with the example.

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
{% for f in site.examples %}{%unless f.noindex%}<tr>
        <td><a href="{{f.url}}">{{f.title}}</a></td><td> {{f.summary}}</td>
</tr>
{%endunless%}{% endfor %}

</table>


---
