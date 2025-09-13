---
layout: default
title: 2
---


## Stage 2
https://xss-quiz.int21h.jp/stage2.php


**What you have to do:**  
Inject the following JavaScript command: `alert(document.domain);`
**Hint:** *close the current tag and add SCRIPT tag...*

As hinted, let's check for open tags; this time, the response page **doesn't display user input.** 
![2.1]({{ site.baseurl }}/assets/images/xss21/2/2.1.png)
However, the input is reflected in the "value" attribute in the form being unsanitized, as it displays `<input type="text" name="p1" size="50" value="<script>alert(document.domain)</script>">`
![2.2]({{ site.baseurl }}/assets/images/xss21/2/2.2.png)
This means that we can close the attribute and the input tag by typing and submitting `"><script>alert(document.domain)</script>` in the textbox.
![2.3]({{ site.baseurl }}/assets/images/xss21/2/2.3.png)