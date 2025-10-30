---
layout: default
title: Level 22 → Level 23
order: 23
---

# [Level 22 → Level 23](https://overthewire.org/wargames/bandit/bandit23.html)
After logging in with 

`ssh bandit22@bandit.labs.overthewire.org -p 2220`

Password: `tRae0UfB9v0UzbCdn9cY0gQnds9GF58Q`

we find out that, just like in [Level 21 → Level 22]({{ site.baseurl }}/bandit/level-21_to_level-22), a program is running automatically at regular intervals from cron, the time-based job scheduler. We have to look through the /etc/cron.d/ directory by using `cd /etc/cron.d/`, once we're in this directory, `ls` shows something like:

```
behemoth4_cleanup  cronjob_bandit22  cronjob_bandit24  leviathan5_cleanup    otw-tmp-dir
clean_tmp          cronjob_bandit23  e2scrub_all       manpage3_resetpw_job  sysstat
```

let's read the content of cronjob_bandit23 with `cat cronjob_bandit23`

```
@reboot bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
* * * * * bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
```

This means that the cronjob_bandit23.sh script in the /usr/bin folder is executed. Let' see what this script does:

`cd /usr/bin` to move into the folder and `cat cronjob_bandit23`. We'll get something along the lines of:

```
#!/bin/bash

myname=$(whoami)
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"

cat /etc/bandit_pass/$myname > /tmp/$mytarget
```

This means that the password file from /etc/bandit_pass/bandit22 is copied into /tmp/[the result of the MD5 hash on bandit22, up to the first space char (-d ' ') (-f 1 stands for the first Field, as the result is separated in two fileds by a space)].

Let' run the same command as we see in the script, with `bandit23` as the result of the `whoami` command (we have to think of ourselves as the bandit23 user, since this script is executed by tha user):

`echo I am user bandit23 | md5sum | cut -d ' ' -f 1`

this will return our destination folder:

`8ca319486bfbbc3663ea0fbe81326349`

let's read the content of our /tmp/8ca319486bfbbc3663ea0fbe81326349 file: ` cat /tmp/8ca319486bfbbc3663ea0fbe81326349`

this will return our password:

> `0Zf11ioIjMVN551jX3CmStKLYqjk54Ga`