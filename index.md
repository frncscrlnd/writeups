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
  {% assign pages_with_numbers = "" | split: "" %}
  
  {% for page in site.yamagata_xss %}
    {% assign page_number = page.title | remove: "Stage " | plus: 0 %}
    {% assign page_with_number = page %}
    {% assign page_with_number.order = page_number %}
    {% assign pages_with_numbers = pages_with_numbers | push: page_with_number %}
  {% endfor %}
  
  {% assign sorted_pages = pages_with_numbers | sort: "order" %}
  {% for page in sorted_pages %}
    <li>
      <a href="{{ site.baseurl }}{{ page.url }}">{{ page.title }}</a>
    </li>
  {% endfor %}
</ul>

---

## Bandit (OverTheWire)

<ul>
  {% assign pages_array = site.bandit | sort: "title" %}
  {% assign sorted_pages = "" | split: "" %}
  
  {% for page in pages_array %}
    {% assign level_number = page.title | remove: 'Level ' | plus: 0 %}
    {% assign page_with_order = page %}
    {% assign page_with_order.order = level_number %}
    {% assign sorted_pages = sorted_pages | push: page_with_order %}
  {% endfor %}
  
  {% assign sorted_pages = sorted_pages | sort: "order" %}
  {% for page in sorted_pages %}
    <li>
      <a href="{{ site.baseurl }}{{ page.url }}">{{ page.title }}</a>
    </li>
  {% endfor %}
</ul>

