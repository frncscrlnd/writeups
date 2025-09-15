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
  {% assign temp_array = "" | split: "," %}
  
  {% for page in site.yamagata_xss %}
    {% assign page_number = page.title | remove: "Stage " | plus: 0 %}
    {% assign temp_item = page | hash: "order", page_number %}
    {% assign temp_array = temp_array | push: temp_item %}
  {% endfor %}
  
  {% assign sorted_pages = temp_array | sort: "order" %}
  
  {% for item in sorted_pages %}
    <li>
      <a href="{{ site.baseurl }}{{ item.url }}">{{ item.title }}</a>
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

