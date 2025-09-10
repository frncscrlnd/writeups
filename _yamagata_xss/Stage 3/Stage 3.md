---
layout: default
title: 3
---
https://xss-quiz.int21h.jp/stage-3.php
**What you have to do:** 
Inject the following JavaScript command: `alert(document.domain);`
**Hint**: *the input in text box is properly escaped.*

The textbox input can not hold any payload anymore; this means we have to turn our attention to the select tag:
![[Pasted image 20250727145259.png]]as we can see, the content ("Japan", in this case) of the select tag is reflected onto the page. We now only need to replace any of the countries into `<script>alert(document.domain);</script>`:
![[Pasted image 20250727145715.png]]
This text will now be displayed in the dropdown menu:
![[Pasted image 20250727145703.png]]
Typing anything in the textbox and submitting will complete the challenge.