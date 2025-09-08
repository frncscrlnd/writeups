---
layout: page
title: 6
permalink: /writeups/xss21/6
---
https://xss-quiz.int21h.jp/stage-no6.php
**What you have to do:**  
Inject the following JavaScript command: `alert(document.domain);`
**Hint:** *event handler attributes.*

Everything in the code **looks** normal, but trying the same payload as [[Stage 2]] (`"><script>alert(document.domain)</script>`) doesn't work:
![[Pasted image 20250727201820.png]]
This means that user input is "html-escaped/encoded" and that the `<` and `>` signs get encoded to `&lt` and `&gt`.
The hint suggests we try to use **event handler attributes** to avoid typing the encoded signs < and >; we'll try using the `onclick` event handler, but you can try `onmouseover`, `onsubmit` or what else you want:
`onclick="alert(document.domain)"`
![[Pasted image 20250727202737.png]]
Clicking (or hovering, if you put on mouse over) on the textbox will complete the challenge.