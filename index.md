---
layout: default
title: Home
---

<style>
ul {
  list-style-type: disc;
  padding-left: 20px;
}

ul li {
  display: list-item; /* assicura che i li siano blocchi */
  margin-bottom: 5px;
}
</style>

# Walkthroughs for thm/htb rooms, writeups and challenges 

## Walkthroughs for TryHackMe

<ul>
  {% for page in site.tryhackme %}
    <li><a href="{{ page.url }}">{{ page.title }}</a></li>
  {% endfor %}
</ul>

## Walkthroughs for XSS challenges (by yamagata21)

<ul>
  {% assign sorted_pages = site.yamagata_xss | sort %}
  {% for page in sorted_pages %}
    {% if page.title == page.title | plus: 0 %}
      <li data-order="{{ page.title }}">
        <a href="{{ page.url }}">{{ page.title }}</a>
      </li>
    {% endif %}
  {% endfor %}
</ul>

