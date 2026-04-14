---
layout: default
title: XSS Challenge by y0n3uchy Replacement
set: Battle with Logic
order: 1
---

# [Replacement](https://xss.challenge.training.hacq.me/challenges/medium01.php)

Let's take a look at this challenge's source code:

```
<script src="hook.js"></script>
<?php
$escaped = preg_replace("/<script>/i", "", $escaped);
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

However, no payload seems to be working