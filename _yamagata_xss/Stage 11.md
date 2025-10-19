---
layout: default
title: Stage 11
order: 11
---

# [Stage 11](http://xss-quiz.int21h.jp/stage11th.php?sid=756e90d9a168c24e2abbc43d1f4409ce6ff70de3)

**What you have to do:**  
Inject the following JavaScript command: `alert(document.domain);`

**Hint:**

This stage is not currently available, not even through [the wayback machine](https://web.archive.org/). 
I got the next level by looking for an URL online. 

A possible solution is 

`"><a href="javascr	ipt:alert(document.domain);">XSS</a>`

as explained [here](https://cheatsheetseries.owasp.org/cheatsheets/XSS_Filter_Evasion_Cheat_Sheet.html#embedded-tab).