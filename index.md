---
layout: default
title: Home
---

# Walkthroughs for thm/htb rooms, writeups and challenges 

<ul>
  {% for page in site.tryhackme %}
    <li><a href="{{ page.url }}">{{ page.title }}</a></li>
  {% endfor %}
</ul>

<h1>Debug: contenuto della collezione</h1>
<ul>
  {% for page in site.tryhackme %}
    <li>{{ page.path }} — {{ page.title }}</li>
  {% endfor %}
</ul>
