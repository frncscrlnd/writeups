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

# Walkthroughs for TryHackMe

<ul>
  {% for page in site.tryhackme %}
    <li><a href="{{ page.url }}">{{ page.title }}</a></li>
  {% endfor %}
</ul>

# Walkthroughs for yamagata21' XSS challenges

<ul>
  {% for page in site.yamagata_xss %}
    <li><a href="{{ page.url }}">{{ page.title }}</a></li>
  {% endfor %}
</ul>