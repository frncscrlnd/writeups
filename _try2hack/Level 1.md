---
layout: default
title: Level 1
order: 1
---

Welcome to this series of write‑ups covering the tr2hack challenges from 2003.

# [Level 1](https://try2hack.nl/levels/)

We are asked to enter a password:

![password prompt]({{ site.baseurl }}/assets/images/t2h/1/1.1.png)

The first level is straightforward: after taking a look at the page's source code, we can spot a cleartext password inside a `<script>` tag that reveals how the page validates the password:

```
<script type="text/javascript">
    <!--
    function Try(passwd) {   
    if (passwd =="h4x0r") {
        alert("Alright! On to level 2...");   
        location.href = "level2-xfdgnh.xhtml";
    }
    else {
        alert("The password is incorrect. Please don't try again.");
        location.href = "http://www.disney.com/";
    }
    }
    //-->  
</script>
```

This means that the password is `h4x0r`