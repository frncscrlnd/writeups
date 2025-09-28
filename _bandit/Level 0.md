---
layout: default
title: Level 0
order: 0
---

{% assign items = site.bandit | sort: "order" %}

{% assign current_index = nil %}
{% for item in items %}
  {% if item.path == page.path %}
    {% assign current_index = forloop.index0 %}
  {% endif %}
{% endfor %}

{% if current_index != nil %}
  {% assign prev_item = items[current_index | minus: 1] %}
  {% assign next_item = items[current_index | plus: 1] %}
  <nav class="nav-bandit">
    {% if prev_item %}
      <a class="prev" href="{{ prev_item.url | relative_url }}">← Precedente: {{ prev_item.title }}</a>
    {% endif %}
    {% if next_item %}
      <a class="next" href="{{ next_item.url | relative_url }}">Successivo: {{ next_item.title }} →</a>
    {% endif %}
  </nav>
{% endif %}

<br>

Welcome to this series of write‑ups covering the Bandit challenges from OverTheWire — a great playground for anyone wanting to sharpen their command line skills, explore Linux fundamentals, and deepen their understanding of basic security concepts.

Bandit is designed to teach you by doing: you connect over SSH, inspect directories, examine hidden files, work with permissions, piping, and other tools of the trade. Each level supplies new puzzles that gradually build on what you’ve already experienced, reinforcing your knowledge through hands‑on practice.

> Tip: Make a list of all passwords as they won't be saved anywhere else

# Level 0

To get to Level 0 you'll need to log in to the target machine (bandit.labs.overthewire.org) using ssh on port 2220. The username is bandit0 and the password is bandit0.
Syntax for ssh connections goes as follows:

`ssh` [username]@[hostname/machine ip] optional (if the port you're connecting to is different from the standard 22):[ -p [port number]]

in our case:
`ssh bandit0@bandit.labs.overthewire.org -p 2220`

Password: `bandit0`

<br>

{% assign items = site.bandit | sort: "order" %}

{% assign current_index = nil %}
{% for item in items %}
  {% if item.path == page.path %}
    {% assign current_index = forloop.index0 %}
  {% endif %}
{% endfor %}

{% if current_index != nil %}
  {% assign prev_item = items[current_index | minus: 1] %}
  {% assign next_item = items[current_index | plus: 1] %}
  <nav class="nav-bandit">
    {% if prev_item %}
      <a class="prev" href="{{ prev_item.url | relative_url }}">← Precedente: {{ prev_item.title }}</a>
    {% endif %}
    {% if next_item %}
      <a class="next" href="{{ next_item.url | relative_url }}">Successivo: {{ next_item.title }} →</a>
    {% endif %}
  </nav>
{% endif %}