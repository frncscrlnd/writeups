---
layout: default
title: Level 21 → Level 22
order: 22
---

# Level 21 → Level 22
After logging in with 

`ssh bandit21@bandit.labs.overthewire.org -p 2220`

Password: `EeoULMCra2q0dSkYj561DX7s1CpBuOBt`

the game tells us to look into the `/etc/cron.d/` directory for the configuration of a scheduled job. `ls` inside this directory shows a `cronjob_bandit22` file:

```
behemoth4_cleanup  cronjob_bandit22  cronjob_bandit24  leviathan5_cleanup    otw-tmp-dir
clean_tmp          cronjob_bandit23  e2scrub_all       manpage3_resetpw_job  sysstat
```

We are gonna use `cat` on this file (`cat cronjob_bandit22`) to se what it does:

```
#!/bin/bash
chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```

this basically changes the permissions on the `/tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv` file adn writes the password from `bandit_22`into it. 

Let's read its content:

`cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv`

This returns our password:

> `tRae0UfB9v0UzbCdn9cY0gQnds9GF58Q`