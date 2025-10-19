---
layout: default
title: Stage 13
order: 13
---

# [Stage 13](https://xss-quiz.int21h.jp/stage13_0.php)

**What you have to do:**  
Inject the following JavaScript command: `alert(document.domain);`

**Hint:** *style attribute*

As you can see there's a `background-color:salmon` attribute inside the form and a `reflect style` button. This means that what you type is reflecte as the value of a `style` attribute. `<`,`>`,`"`,`'` and `&` characters are escaped.