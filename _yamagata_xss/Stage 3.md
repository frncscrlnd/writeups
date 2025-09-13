---
layout: default
title: 3
---


# Stage 3
https://xss-quiz.int21h.jp/stage-3.php


**What you have to do:** 
Inject the following JavaScript command: `alert(document.domain);`
**Hint**: *the input in text box is properly escaped.*

The textbox input can not hold any payload anymore; this means we have to turn our attention to the select tag:
![3.1]({{ site.baseurl }}/assets/images/xss21/3/3.1.png)

as we can see, the content ("Japan", in this case) of the select tag is reflected onto the page. We now only need to replace any of the countries into `<script>alert(document.domain);</script>`:
![3.2]({{ site.baseurl }}/assets/images/xss21/3/3.2.png)


This text will now be displayed in the dropdown menu:
![3.3]({{ site.baseurl }}/assets/images/xss21/3/3.3.png)


Typing anything in the textbox and submitting will complete the challenge.