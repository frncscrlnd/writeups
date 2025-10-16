---
layout: default
title: Stage 12
order: 12
---

# [Stage 12](http://xss-quiz.int21h.jp/stage_no012.php)

**What you have to do:**  
Inject the following JavaScript command: `alert(document.domain);`
**Hint:** *"s/[\x00-\x20\<\>\"\']//g;"*

The hint suggests a **regex** rule is used to sanitize user input. Specifically, this rule searches the input text for:

- the first 32 ASCII chars (`\x00-\x20`), including NUL, TAB and Space
- `<` `>` chars (`\<\>`)
- double quotes `"` (`\"`)
- `'` single quote (`\'`)

and replaces (`s`) them with a void string (`//`). This means that we can not use any of these chars.

This would make it impossible to submit a payload if only the backtick (`` ` ``) character didn't exist: this character will allow us to close the input text's `value` attribute and submit a new onmouseover attribute: ``` ``onmouseover=alert(document.domain); ```

![12.1]({{ site.baseurl }}/assets/images/xss21/12/12.1.png)

like this:

![12.2]({{ site.baseurl }}/assets/images/xss21/12/12.2.png)

we can also use ``` ``onclick=alert(document.domain);  ```