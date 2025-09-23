---
layout: default
title: Level 19 → Level 20
order: 20
---

# Level 19 → Level 20
After logging in with 

`ssh bandit19@bandit.labs.overthewire.org -p 2220`

Password: `cGWpMaKXVwDUNgPAVJbWYuGHVn9zl3j8`

that there's a bandit20-do file in the home/bandit19 directory. We have to use this setuid binary. Running ./bandit20-do will return how: 

`Run a command as another user.
Example: ./bandit20-do id`

This means that this file belongs to the bandit20 user but we can use it (we're bandit 19).

Running `ls -l` (this command returns the permissions for the user, group and others) proves this:

`-rwsr-x--- 1 bandit20 bandit19 14884 Aug 15 13:16 bandit20-do`

Let's use bandit20-do to run command as if we were bandit20, this means accessing the password at `/etc/bandit_pass/bandit20`:

`bandit20-do cat /etc/bandit_pass/bandit20`

this will return our password for the next level:

> `0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO`