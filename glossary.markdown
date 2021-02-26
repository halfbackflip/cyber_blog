---
layout: "page"
title: Glossary
permalink: /glossary
---

This page tracks all the terms, acronyms and tech slang I run across.

<table>
{% for item in site.data.cysaglossary %}
    <tr>
        <td>{{item.Term}}</td> 
        <td>{{item.Definition}}</td>
    </tr>
{% endfor %}
</table>


<!-- {{site.data.cysaglossary}} -->