---
layout: default
title: Stage 1
order: 1
---


The XSS Challenges by Yamagata21 constitute one of the pioneering and educational XSS (Cross‑Site Scripting) challenge sets available, dating back to 2008. Comprised of 20 successive stages (though a few later ones may only function properly in legacy environments like older versions of Internet Explorer), the series begins with straightforward reflected XSS injections using `alert(document.domain)` and gradually progresses towards less intuitive scenarios.

## Stage 1
https://xss-quiz.int21h.jp/


**What you have to do:**  
Inject the following JavaScript command: `alert(document.domain);`
**Hint:** *very simple...*

If characters like `<`, `>`, or `&` are submitted, they are displayed exactly as entered (e.g., _"No results for **"<"**"_, when it should be _"No results for **"<"**_").  
![first]({{ site.baseurl }}/assets/images/xss21/1/1.png)
This means that user input is not sanitized and that JavaScript code such as `<script>alert(document.domain)</script>` can be submitted and potentially executed in the user's browser.