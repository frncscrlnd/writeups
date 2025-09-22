---
layout: default
title: Level 14 → Level 15
order: 15
---

# Level 14 → Level 15
After logging in with 

`ssh bandit14@bandit.labs.overthewire.org -p 2220`

Password: `MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS`

we see by running `ls` that there are no files in the home directory. As the game tells us, we have to submit via netcat (`nc`) the password of the current level to port 3000 on localhost. As [Level 13 → Level 14]({{ site.baseurl }}/bandit/level-13_to_level-14) tells us, the password is in the /etc/bandit_pass/bandit14 directory: `cat /etc/bandit_pass/bandit14` returns: `MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS`

Now we can submit our password via nc. Let's use:

`nc localhost 30000`

And now paste and submit the password.

This will return:

"Correct!"

> `8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo`