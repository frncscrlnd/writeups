---
layout: default
title: XSS Challenge by y0n3uchy Baby XSS 04
set: For Newbies
order: 4
---

# [Baby XSS 04](https://xss.challenge.training.hacq.me/challenges/baby04.php)

Let's take a look at this page's source code:

```
<script src="hook.js"></script>
<?php
// by escaping the payload you won't break this system, haha! :-)
$escaped = preg_replace("/[`<>ux]\\/", "", $_GET['payload']);
?>

<script>
    window.addEventListener("load", function() {
        var name = `<?= $escaped ?>`;
        window.greeting.innerHTML = (name == '' ? 'こんにちは!' : name + 'さん, こんにちは!');
    });
</script>

<p id="greeting"></p>

<h1>inject</h1>
<form>
    <input type="text" name="payload" placeholder="your payload here">
    <input type="submit" value="GO">
</form>

<h1>src</h1>
<?php highlight_string(file_get_contents(basename(__FILE__))); ?>
```

However, no payload is visible after submitting. Il will always return こんにちは!