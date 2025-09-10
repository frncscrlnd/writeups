---
layout: default
title: 8
---
[← Back to the Home page]({{ site.baseurl }}/)


# Stage 8
https://xss-quiz.int21h.jp/stage008.php
**What you have to do:**  
Inject the following JavaScript command: `alert(document.domain);`
**Hint:** *the 'javascript' scheme*.

This page turns user input into a URL. We'll follow the hint and type a JavaScript URI like this: 
`javascript:alert(document.domain);`:
![[Pasted image 20250727231045.png]]
An URL will now be displayed:
![[Pasted image 20250727231118.png]]
Clicking on it solves the challenge.
