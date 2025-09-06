---
layout: default
title: Home
---

# I miei Writeups

<ul>
  {% for page in site.pages %}
    {% if page.path contains 'writeups/' %}
      <li><a href="{{ page.url }}">{{ page.title }}</a></li>
    {% endif %}
  {% endfor %}
</ul>