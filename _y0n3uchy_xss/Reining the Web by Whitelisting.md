---
layout: default
title: XSS Challenge by y0n3uchy Reining the Web by Whitelisting
set: Battle with Content-Security-Policy
order: 1
---

# [Reining the Web by Whitelisting](https://xss.challenge.training.hacq.me/challenges/csp01.php)

When loading this challenge a `預金残高: 30000` alert will pop up. 預金残高 means "deposit balance" in Japanese.

Let's look at this level's source code:

```
<?php header("Content-Security-Policy: default-src 'self'; style-src 'unsafe-inline'"); ?>
<script src="hook.js"></script>


<script src="csp01-util.js"></script>
<script src="csp01-jsonp.php?callback=callback"></script>

<h1>Hello, <?= $_GET['payload'] ?>!</h1>

<h1>inject</h1>
<form>
    <input type="text" name="payload" placeholder="your payload here">
    <input type="submit" value="GO">
</form>

<h1>src</h1>
<?php highlight_string(file_get_contents(basename(__FILE__))); ?>
```

As you may have noticed, this challenge is the first of a set that is dedicated entirely to [Content Security Policy (CSP)](https://en.wikipedia.org/wiki/Content_Security_Policy). CSP is an [HTTP header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers) which restricts the locations from which you can load resources. 

In our case, `<?php header("Content-Security-Policy: default-src 'self'; style-src 'unsafe-inline'"); ?>` means that any resource must be loaded only from the same domain (`default-src 'self';`) and that inline CSS is allowed (`style-src 'unsafe-inline'`) as an exception to the `default-src` rule.

We can also see that there are two `script` tags:

```
<script src="csp01-util.js"></script>
<script src="csp01-jsonp.php?callback=callback"></script>
```

these two will work as intended because both resources (`csp01-util.js` and `csp01-jsonp.php`) are on this domain. Let's take a look at these files:

`csp01-util.js`

```
function callback(money){
    alert(`é é‡‘æ®‹é«˜: ${money}`);
}
```

in this file a function which displays an alert is defined. This is what we see upon loading the challenge.

`csp01-jsonp.php`

```
callback(30000);
```

this other file calls the function. 

This works not only because both resources are on the same domain, but also because it uses [JSONP](https://en.wikipedia.org/wiki/JSONP), a technique (that has now been replaced by [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/Guides/CORS)) which allows to bypass the browser's [Same Origin Policy](https://en.wikipedia.org/wiki/Same-origin_policy) that prevents other website's from accessing a website's data. 

JSONP works by exploiting the fact that the `<script>` tag can load resources from anywhere (that's why this website uses a CSP). It appends the name of the function to call to the URL like this `?callback=function` or, in this case, `?callback=callback`.

We can exploit this by changing which function is being called to standard JS:

>`<script src="csp01-jsonp.php?callback=alert(document.domain)"></script>`

submitting this payload will solve the challenge