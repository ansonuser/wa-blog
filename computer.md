---
title: Computer
layout: default
categories: Main
permalink: /computer.html
---

# 目錄

<html>


{% for post in site.categories.Computer %}   
    <li style="padding: 5px;"><a href="{{post.url | prepend:site.baseurl}}">{{post.title}}</a> </li>
{% endfor %}
</html>