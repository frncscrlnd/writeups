---
layout: default
title: Stage 4
order: 4
---


# [Stage 4](https://xss-quiz.int21h.jp/stage_4.php)

**What you have to do:**  
Inject the following JavaScript command: `alert(document.domain);`

**Hint:** *invisible input field.*

While looking a the code we stumble upon an "**hidden**" input field, as the hint suggests:
![4.1]({{ site.baseurl }}/assets/images/xss21/4/4.1.png)

Let's change the type back to  `"text"`:
![4.2]({{ site.baseurl }}/assets/images/xss21/4/4.2.png)

A textbox will now be reflected in the webpage:
![4.3]({{ site.baseurl }}/assets/images/xss21/4/4.3.png)

We'll now deliver the payload through this textbox as we did for [Stage 2]({{ site.baseurl }}xss/Stage-2/):
`"><script>alert(document.domain)</script>`.