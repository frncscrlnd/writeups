---
layout: default
title: XSS Challenge by y0n3uchy No Parentheses
set: Battle with Filters
order: 2
---

# [No Parentheses](https://xss.challenge.training.hacq.me/challenges/easy02.php)

Let's start by looking at the page's source code:

```
<script src="hook.js"></script>
<?php
// you cannot do anything without ...

// no parentheses ...
$escaped = preg_replace("/[()]/", "", $_GET['payload']);

// no event handlers!
$escaped = preg_replace("/.*o.*n.*/i", "", $escaped);
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

Just like the [last challenge](https://frncscrlnd.github.io/writeups/y0n3uchy_xss/No-Alphabets-and-Digits/), `preg_replace` is being used to sanitize input. This time it has been done twice:

```
// no parentheses ...
$escaped = preg_replace("/[()]/", "", $_GET['payload']);

// no event handlers!
$escaped = preg_replace("/.*o.*n.*/i", "", $escaped);
```

This means that we cannot use the following characters: `(` and `)` as they will be replaced with `""`

Also, any string that contains the `on` will be removed (replaced with `""`).

However we can still easily get through this filter with the `` ` `` (backtick) character by using [tagged templates](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals#tagged_templates): the `` ` `` can, in fact, act like a `(` parenthesis:

`<script>alert("XSS")</script>` 

is equal to

>``<script>alert`XSS`</script>``
