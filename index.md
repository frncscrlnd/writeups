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

summary:hover{
  cursor: pointer;
}
</style>


# Stuck on a challenge/room/CTF?
<br>
## [TryHackMe](https://tryhackme.com/)
<div style="overflow: hidden; height: 80px; flex-shrink: 0;">
  <iframe
    src="https://tryhackme.com/api/v2/badges/public-profile?userPublicId=3695567"
    width="480"
    height="80"
    scrolling="no"
    style="border: none; display: block;">
  </iframe>
</div>
<details>
  <summary>> show/hide</summary>
  <ul>
  {% for page in site.tryhackme %}
    <li>
      <a href="{{ site.baseurl }}{{ page.url }}">{{ page.title }}</a>
    </li>
  {% endfor %}
</ul>
</details>
<hr>
## [Hack The Box](https://www.hackthebox.com)
<a href="https://app.hackthebox.com/users/1392650" target="_blank" style="flex-shrink: 0; line-height: 0; padding: 0;">
    <img
      src="https://www.hackthebox.com/badge/image/1392650"
      alt="Hack The Box Profile"
      height="80"
    />
  </a>
  <details>
    <summary>> show/hide</summary>
      <ul>
        {% for page in site.hackthebox %}
          <li>
            <a href="{{ site.baseurl }}{{ page.url }}">{{ page.title | remove: "Hack The Box " }}</a>
          </li>
        {% endfor %}
      </ul>
  </details>
<hr>
## [XSS challenges (by yamagata21)](https://xss-quiz.int21h.jp/)
<details>
  <summary>> show/hide</summary>
    <ul>
      {% assign sorted_xss = site.yamagata_xss | sort: "order" %}
      {% for page in sorted_xss %}
      <li><a href="{{ site.baseurl }}{{ page.url }}">{{ page.title | remove: "XSS Challenges (by yamagata21) " }}</a></li>
      {% endfor %}
    </ul>
</details>
<hr>
## [Bandit (OverTheWire)](https://overthewire.org/wargames/bandit/)
<details>
  <summary>> show/hide</summary>
  <ul>
    {% assign sorted_bandit = site.bandit | sort: "order" %}
    {% for page in sorted_bandit %}
      <li><a href="{{ site.baseurl }}{{ page.url }}">{{ page.title }}</a></li>
    {% endfor %}
  </ul>
</details>
<hr>
## [the cryptopals crypto challenges](https://cryptopals.com/)
<details>
  <summary>> show/hide</summary>
  {% assign sorted_cryptopals = site.cryptopals | sort: "order" %}
  {% assign grouped_sets = sorted_cryptopals | group_by: "set" %}
  {% for set in grouped_sets %}
    <details>
      <summary>&nbsp;&nbsp;&nbsp;&nbsp;> Set {{ set.name }}</summary>
      <ol>
        {% for page in set.items %}
          <li>
            <a href="{{ site.baseurl }}{{ page.url }}">{{ page.title | remove: "the cryptopals crypto challenges " }}</a>
          </li>
        {% endfor %}
      </ol>
    </details>
  {% endfor %}
</details>
<hr>
## [try2hack.nl](https://try2hack.nl)
<details>
  <summary>> show/hide</summary>
  <ul>
    {% assign sorted_try2hack = site.try2hack | sort: "order" %}
    {% for page in sorted_try2hack %}
      <li><a href="{{ site.baseurl }}{{ page.url }}">{{ page.title | remove: "try2hack " }}</a></li>
    {% endfor %}
  </ul>
</details>
<hr>