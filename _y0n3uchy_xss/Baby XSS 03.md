---
layout: default
title: XSS Challenge by y0n3uchy Baby XSS 03
set: For Newbies
order: 3
---

# [Baby XSS 03](https://xss.challenge.training.hacq.me/challenges/baby03.php)

Just like other challenges on this website, the page's backend source is visible:

```
<script src="hook.js"></script>
<?php
$escaped = htmlspecialchars($_GET['payload']);
?>

<h1>Hello, <?= $escaped ?></h1>
<a href="<?= $escaped ?>/friends">Friends</a>
<a href="<?= $escaped ?>/post">Posts</a>
<a href="<?= $escaped ?>/settings">Settings</a>

<h1>inject</h1>
<form>
    <input type="text" name="payload" placeholder="your payload here">
    <input type="submit" value="GO">
</form>

<h1>src</h1>
<?php highlight_string(file_get_contents(basename(__FILE__))); ?>
```

This time the text input form is back, meaning we'll have to submit our payload there. The payload will, however, go through `htmlspecialchars` before being inserted into the link's `href` attribute. This means that the following encoding will be applied to these characters:

```
& &amp;
< &lt;
> &gt;
" &quot;
' &#039; 
```

That means that this time we'll have to use a new way to implement javascript code in our input: `javascript:`

This tag allows us to include js in `href` attributes (just like `mailto:` elements).

>`javascript:alert(document.domain)`

will solve the challenge