---
layout: page
title: 5
permalink: /xss21/5
---
https://xss-quiz.int21h.jp/stage--5.php
**What you have to do:**  
Inject the following JavaScript command: `alert(document.domain);`
**Hint:** *length limited text box.*

While looking thtough the code we notice that the textbox has a `maxlength` attribute set to 15:
![[Pasted image 20250727161150.png]]
Let's change that back to a useful value:
![[Pasted image 20250727161220.png]]
We'll now deliver the payload through this textbox as we did for [[Stage 2]]:
`"><script>alert(document.domain)</script>`.