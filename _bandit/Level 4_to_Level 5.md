---
layout: default
title: Level 4 → Level 5
order: 5
---

# Level 4 → Level 5
After logging in with 

`ssh bandit4@bandit.labs.overthewire.org -p 2220`

Password: `2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ`

The `ls` command shows the inhere folder, which is the target of our research. Moving into this directory with `cd` and by showing its' content with `ls` again, we'll see that there are 9 files in here:

`-file00  -file01  -file02  -file03  -file04  -file05  -file06  -file07  -file08  -file09`

By running `cat` on each one of them (remember to use the full filename that starts with `./` e.g. `cat ./-file00`) we'll realize that only human-readable file is `-file07`, and it contains our password:

> `4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw`