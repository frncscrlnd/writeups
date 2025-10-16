---
layout: default
title: Stage 9
order: 9
---


# [Stage 9](https://xss-quiz.int21h.jp/stage_09.php)

**What you have to do:**  
Inject the following JavaScript command: `alert(document.domain);`
**Hint:** *UTF-7 XSS*.

Since this is a UTF-7 related XSS, most browser's won't support it. Learn more [here](https://en.wikipedia.org/wiki/UTF-7#Security). Also check this [resource](https://gihyo.jp/admin/serial/01/charcode/0001) out. 

You can skip it by opening the console and typing `alert(document.domain);`. If you want to solve this, open Internet Explorer (version 8 or below, i used 5) and use this payload `+ACI-+AD4-+ADw-script+AD4-alert(document.domain)+ADs-+ADw-/script+AD4-` (which is `"><script>alert(document.domain);</script>` encoded as UTF-7). Then, change the `value` attribute in the `charset` input tag from `euc-jp`

![9.1]({{ site.baseurl }}/assets/images/xss21/9/9.1.png)

to `utf-7`:

![9.2]({{ site.baseurl }}/assets/images/xss21/9/9.2.png)

Then, submit the form.