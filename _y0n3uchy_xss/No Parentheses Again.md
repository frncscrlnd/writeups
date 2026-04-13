---
layout: default
title: XSS Challenge by y0n3uchy No Parentheses Again
set: Battle with Filters
order: 4
---

# [No Parentheses Again](https://xss.challenge.training.hacq.me/challenges/easy04.php)

Let's take a look at this level's source code:

```
<script src="hook.js"></script>
<?php
$escaped = preg_replace("/[`()<>&#]/", "", $_GET['payload']);
?>

<h1>Hello, <span id="<?= $escaped ?>"><?= htmlspecialchars($_GET['payload']) ?></span>!</h1>

<h1>inject</h1>
<form>
    <input type="text" name="payload" placeholder="your payload here">
    <input type="submit" value="GO">
</form>

<h1>src</h1>
<?php highlight_string(file_get_contents(basename(__FILE__))); ?>
```

This means that we can't use the `` ` ``, `(`, `)`, `<`, `>`, `&` and `#` characters. Also, since the payload appears twice, the second time our original input will be sanitized like we've seen in [this challenge](https://frncscrlnd.github.io/writeups/y0n3uchy_xss/Baby-XSS-03/). Again, `htmlsspecialchars` simply encodes the characters like this:

```
& &amp;
< &lt;
> &gt;
" &quot;
' &#039; 
```

Last time out, we used the `javascript:` attribute like this `javascript:alert(document.domain)`. This time we can't use it since we can not use `(` and `)`. What is not sanitized from previous challenges, however, are event handlers such as [`onclick`](https://www.w3schools.com/jsref/event_onclick.asp), [`onmouseover`](https://www.w3schools.com/jsref/event_onmouseover.asp) and [`onfocus`](https://www.w3schools.com/jsref/event_onfocus.asp). 