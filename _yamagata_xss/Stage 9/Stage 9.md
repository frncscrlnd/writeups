---
layout: page
title: 9
permalink: /writeups/xss21/9
---
https://xss-quiz.int21h.jp/stage_09.php
**What you have to do:**  
Inject the following JavaScript command: `alert(document.domain);`
**Hint:** *UTF-7 XSS*.

Since this is a UTF-7 related XSS, most browser's won't support it. Learn more [here](https://en.wikipedia.org/wiki/UTF-7#Security).
You can skip it by opening the console and typing `alert(document.domain);`. If you want to solve this, open Internet Explorer (version 8 or below) and use this payload `+ACI- onmouseover=+ACI-alert(document.domain)+ADsAIg-` (`" onmouseover="alert(document.domain);"` encoded as UTF-7).
