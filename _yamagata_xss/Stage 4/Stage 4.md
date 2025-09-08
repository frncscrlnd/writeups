---
layout: page
title: 4
---
https://xss-quiz.int21h.jp/stage_4.php
**What you have to do:**  
Inject the following JavaScript command: `alert(document.domain);`
**Hint:** *invisible input field.*

While looking a the code we stumble upon an "**hidden**" input field, as the hint suggests:
![[Pasted image 20250727155342.png]]
Let's change the type back to  `"text"`:
![[Pasted image 20250727160421.png]]
A textbox will now be reflected in the webpage:
![[Pasted image 20250727160457.png]]
We'll now deliver the payload through this textbox as we did for [[Stage 2]]:
`"><script>alert(document.domain)</script>`.