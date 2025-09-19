---
layout: default
title: Level 5 → Level 6
order: 6
---

# Level 5 → Level 6
After logging in with 

`ssh bandit5@bandit.labs.overthewire.org -p 2220`

Password: `4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw`

After `cd`ing into inhere we can see that ther's multiple directories. Instead of viewing each one of them (since each directory hs multiple files), we can search for what the game tells us to find: 

- human-readable; 
- 1033 bytes in size;
- not an executable file.

We can do this with this command: `find inhere -type f -size 1033c`

This will only return files (`-type f`) that are 1033bytes (`-size 1033c`) in size. in our case: 

`inhere/maybehere07/.file2`

By using `cat` on `inhere/maybehere07/.file2` we can see that the password is:

> `HWasnPhtq9AVKe0dmk45nxy20cvUa6EG`