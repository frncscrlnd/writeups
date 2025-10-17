---
layout: page
title: Stage 7
order: 7
---

# [Stage 7](https://xss-quiz.int21h.jp/stage07.php)

**What you have to do:**  
Inject the following JavaScript command: `alert(document.domain);`

**Hint:** *nearly the same... but a bit more tricky.*

The code looks fine; since the hint tells us this challenge is nearly the same as [[Stage 6]], we'll have to look for some kind of escaping or encoding, not only to the textbox input, but also to tags and attirbutes. However, typing the same payload as [[Stage 6]] (`onclick="alert(document.domain)"`) after the `value` attribute and clicking (or hovering for `onmouseover`) will complete the challenge:
![7.1]({{ site.baseurl }}/assets/images/xss21/7/7.1.png)



