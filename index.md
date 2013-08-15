---
layout: page
title: Welcome! (.NET)
groups:
  - name: Getting Started
  - name: General Use
  
---
{% include JB/setup %}

Awesomium provides everything you need to start displaying beautiful HTML-powered interfaces and web-content within your C++ or .NET application fast.

It takes only a minute to get started, begin by following the tutorials below.

You are at the <span class="highlight">.NET Language</span> Wiki, to view the C++ Language Wiki please [click here](http://wiki.awesomium.com). All articles are compatible with version 1.7+ only.

{% for g in site.groups %}
### {{ g.name }}
<ul class="truncate">{% assign pages_list = site.pages %}{% assign group = g.name %}{% include JB/pages_list %}</ul>
{% endfor %}
