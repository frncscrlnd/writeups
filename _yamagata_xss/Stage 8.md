---
layout: default
title: Stage 8
order: 8
---


# Stage 8
https://xss-quiz.int21h.jp/stage008.php


**What you have to do:**  
Inject the following JavaScript command: `alert(document.domain);`
**Hint:** *the 'javascript' scheme*.

This page turns user input into a URL. We'll follow the hint and type a JavaScript URI like this: 
`javascript:alert(document.domain);`:
![8.1]({{ site.baseurl }}/assets/images/xss21/8/8.1.png)

An URL will now be displayed:
![8.2]({{ site.baseurl }}/assets/images/xss21/8/8.2.png)

Clicking on it solves the challenge.
