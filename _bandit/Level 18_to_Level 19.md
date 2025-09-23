---
layout: default
title: Level 18 → Level 19
order: 19
---

# Level 18 → Level 19
After logging in with 

`ssh bandit18@bandit.labs.overthewire.org -p 2220`

Password: `x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO`

...no, wait. Something's wrong, the ssh login doesn't work: as the game tells us, someone has modified .bashrc to log you out when you log in with SSH. Since we can't login with SSH, we'll try using the -T flag, which disables the shell:

`ssh -T -p 2220 bandit@bandit.labs.overthewire.org`

This returns a blinking cursor:

Let's read home/bandit18/readme:

`cat readme`

> `cGWpMaKXVwDUNgPAVJbWYuGHVn9zl3j8`


