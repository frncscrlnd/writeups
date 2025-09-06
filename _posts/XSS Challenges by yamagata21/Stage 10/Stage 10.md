---
Layout: page
Title: 10
Permalink: /xss21/10
---
https://xss-quiz.int21h.jp/stage00010.php
**What you have to do:**  
Inject the following JavaScript command: `alert(document.domain);`
**Hint:** *s/domain//g*

The hint suggests a **regex** rule is used to sanitize user input. Specifically, this rule searches for the `domain` string in the text and replaces (`s`) it with a void string (`//`). This rule applies to all of the user submitted text (`g`).
A workaround to this is to split the `domain` string in half and put the two halves around another `domain` string: `domdomainain`, `ddomainomain, dodomainmain, domadomainin, domaidomainn` are all valid alternatives. Let's pick `domadomainin`.
After typing `"><script>alert(document.domain)</script>` we get this:
![[Pasted image 20250728173255.png]]
This means we have to deliver our payload (`"><script>alert(document.domadomainin)</script>`) in the textbox because special characters are not escaped.