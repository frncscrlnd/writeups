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
  {% assign sorted_pages = site.yamagata_xss | sort: "title" %}
  {% assign sorted_pages = sorted_pages | sort: "page_number" %}
  {% for page in sorted_pages %}
    {% assign page_number = page.title | plus: 0 %}
    {% if page_number == page_number %} 
      <li data-order="{{ page_number }}">
        <a href="{{ site.baseurl }}{{ page.url }}">{{ page.title }}</a>
      </li>
    {% endif %}
  {% endfor %}
</ul>

---

## Bandit (OverTheWire)

<ul>
  {% assign sorted_bandit = site.bandit | sort: "title" %}
  {% for page in sorted_bandit %}
    <li>
      <a href="{{ site.baseurl }}{{ page.url }}">{{ page.title }}</a>
    </li>
  {% endfor %}
</ul>

