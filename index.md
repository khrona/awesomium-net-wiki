---
layout: page
title: Welcome to the Awesomium.NET Wiki
groups:
  - name: Getting Started
  - name: General Use
  
---
{% include JB/setup %}

Awesomium provides everything you need to start displaying beautiful HTML-powered interfaces and web-content within your C++ or .NET application fast.

It takes only a minute to get started, begin by following the tutorials below.

You are at the <span class="highlight">Awesomium.NET</span> Wiki, to view the C++ Language Wiki please [click here](http://wiki.awesomium.com). All articles are compatible with version 1.7+ only.

{% for g in site.groups %}
### {{ g.name }}
<ul class="truncate">{% assign pages_list = site.pages %}{% assign group = g.name %}{% if g.name == 'Changelogs' %}<li><a href='./getting-started/whats-new.html'>What's New in 1.7.1</a></li>{% endif %}{% include JB/pages_list %}</ul>
{% endfor %}