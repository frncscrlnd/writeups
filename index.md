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
  display: list-item; 
  margin-bottom: 5px;
}
</style>

# Stuck on a challenge/room/CTF?
<br>

## TryHackMe

<ul>
  {% for page in site.tryhackme %}
    <li>
      <a href="{{ site.baseurl }}{{ page.url }}">{{ page.title }}</a>
    </li>
  {% endfor %}
</ul>

---

## XSS challenges (by yamagata21)

<ul>
  {% assign sorted_pages = site.yamagata_xss | sort: "order" %}
  {% for page in sorted_pages %}
    <li>
      <a href="{{ site.baseurl }}{{ page.url }}">{{ page.title }}</a>
    </li>
  {% endfor %}
</ul>

---

## Bandit (OverTheWire)

<ul>
  {% assign sorted_bandit_pages = site.bandit | sort: "order" %}
  {% for page in sorted_bandit_pages %}
    <li>
      <a href="{{ site.baseurl }}{{ page.url }}">{{ page.title }}</a>
    </li>
  {% endfor %}
</ul>

