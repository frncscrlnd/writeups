---
layout: default
title: XSS Challenge by y0n3uchy Frustrating App
set: Battle with Content-Security-Policy
order: 4
---

# [Frustrating App](https://xss.challenge.training.hacq.me/challenges/csp04.php)

Let's take a look at this challenge's source code:

```
<?php
$escaped = preg_replace("/[`$<>]/", "", $_GET['payload']);

$nonce = base64_encode(random_bytes(20));
header("script-src 'strict-dynamic' 'nonce-" . $nonce . "' 'unsafe-eval';");
?>

<head>
    <script src="hook.js" nonce="<?= $random2 ?>"></script>
</head>

<body>
    <script nonce="<?= $random ?>">
        window.addEventListener("load", function() {
            var input = `<?= $escaped ?>`;
            window.injectarea.innerHTML = `${input} is your payload; Could you execute a script? :-)`
        });
    </script>

    <h1>Your raw payload</h1>
    <?= $_GET['payload'] ?>

    <div id="injectarea"></div>
    <h1>inject</h1>
    <form>
        <textarea id="payload" name="payload" placeholder="your payload here"></textarea>
        <input type="submit" value="GO">
    </form>

    <h1>src</h1>
    <?php highlight_string(file_get_contents(basename(__FILE__))); ?>
</body>
```

however, any payload will return error 502