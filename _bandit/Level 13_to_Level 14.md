---
layout: default
title: Level 13 → Level 14
order: 14
---

# [Level 13 → Level 14](https://overthewire.org/wargames/bandit/bandit14.html)
After logging in with 

`ssh bandit13@bandit.labs.overthewire.org -p 2220`

Password: `FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn`

we can see the `sshkey.private private ssh key by running `ls` on the home directory. According to the game, we have to access the next level [Level 14 → Level 15]({{ site.baseurl }}/bandit/level-14_to_level-15) by logging in via ssh key to the bandit14 user on the localhost machine. Since we already have the private key, the syntax for our login will be:

`ssh -p 2220 -i sshkey.private bandit14@localhost`

Then we just need to follow the instruction given by the game adn read the content of `/etc/bandit_pass/bandit14`:

`cat /etc/bandit_pass/bandit14`

This will give us the password for the next level:

> `MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS`