---
layout: default
title: Level 6 → Level 7
order: 7
---

# Level 6 → Level 7
After logging in with 

`ssh bandit6@bandit.labs.overthewire.org -p 2220`

Password: `HWasnPhtq9AVKe0dmk45nxy20cvUa6EG`

We see, by running -ls, that no file is here. We'll have to look at the machine's root (`/`) directory with `cd /`. Just like [Level 5 → Level 6]({{ site.baseurl }}/bandit/level-5_to_level-6) we have some info about our target file: 

- it's owned by user bandit7;
- it's owned by group bandit6;
- it's 33 bytes in size.

That means, respectively:
That means that if we run ls -l on the file's directory, we'll see: 

`-rw-r--r-- 1 bandit7 bandit6 46 Apr 14 16:37 target.txt` this is because the first name represents the user who owns the file, while second the group it belongs to.

Just like we did in [Level 5 → Level 6]({{ site.baseurl }}/bandit/level-5_to_level-6) we'll have to use the `find` command:

`find / -type f -user bandit7 -group bandit6 -size 33c`

We'll get, among others (you can also turn off `permission denied` messages with `2>/dev/null`), this file: `./var/lib/dpkg/info/bandit7.password`

Let's run 

`cat ./var/lib/dpkg/info/bandit7.password`


And we'll get the password for the next level:

> `morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj`

