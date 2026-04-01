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
<div style="display: flex; align-items: center; gap: 24px; flex-wrap: wrap; margin-bottom: 16px;">
  <div style="overflow: hidden; height: 80px;">
    <iframe
      src="https://tryhackme.com/api/v2/badges/public-profile?userPublicId=3695567"
      width="480"
      height="80"
      scrolling="no"
      style="border: none; display: block;">
    </iframe>
  </div>
  <a href="https://app.hackthebox.com/users/1392650" target="_blank">
    <img
      src="https://www.hackthebox.com/badge/image/1392650"
      alt="Hack The Box Profile"
      height="40"
    />
  </a>
  <a href="https://pwn.college/hacker/frncscrlnd" target="_blank">pwn.college profile</a>
</div>
<br>
## [TryHackMe](https://tryhackme.com/)
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
## [XSS challenges (by yamagata21)](https://xss-quiz.int21h.jp/)
<details>
  <summary>> show/hide</summary>
    <ul>
      {% assign sorted_xss = site.yamagata_xss | sort: "order" %}
      {% for page in sorted_xss %}
      <li><a href="{{ site.baseurl }}{{ page.url }}">{{ page.title }}</a></li>
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
            <a href="{{ site.baseurl }}{{ page.url }}">{{ page.title }}</a>
          </li>
        {% endfor %}
      </ol>
    </details>
  {% endfor %}
</details>
