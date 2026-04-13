---
layout: default
title: XSS Challenge by y0n3uchy No Quotes
set: Battle with Filters
order: 3
---

# [No Quotes](https://xss.challenge.training.hacq.me/challenges/easy03.php)

Let's take a look at this challenge's source code:

```
<script src="hook.js"></script>
<?php
// by escaping the payload you won't break this system, haha! :-)
$escaped = preg_replace("/['\"`&#]/", "", $_GET['payload']);
?>

<h1>Hello, <?= $escaped ?>!</h1>

<h1>inject</h1>
<form>
    <input type="text" name="payload" placeholder="your payload here">
    <input type="submit" value="GO">
</form>

<h1>src</h1>
<?php highlight_string(file_get_contents(basename(__FILE__))); ?>
```

This time the `preg_replace` function we saw in [previous challenges](https://frncscrlnd.github.io/writeups/y0n3uchy_xss/No-Alphabets-and-Digits/) is now used to replace the `'`, `"`, `` ` ``, `&` and `#` characters with `""`.

This forces us into using no quotes, but thats' not a problem for us: the payload from the frost challenge will work perfcetly for this one as it does not have any quotes.

>`<script>alert(document.domain)</script>`

