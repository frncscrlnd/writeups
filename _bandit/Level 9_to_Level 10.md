---
layout: default
title: Level 9 → Level 10
order: 10
---

# Level  → Level 10
After logging in with 

`ssh bandit9@bandit.labs.overthewire.org -p 2220`

Password: `4CKMh1JI91bUIZZPXDqGanal4xvAg0JM`

By running `ls` we can see that there is a data.txt file inside the /home/bandit9 directory. The content of this file is not human readable, as we can see by running cat on it. The game also tells us our password is preceded by several = characters. 

To make the content of data.txt readable, we'll need `strings` as this command only prints human-readable characters: `strings data.txt`. But running this is not good enough: we need to get the string that is preceded by `=` characters. To make this possible, we'll use `grep`. 

Let's try this pipeline: 

`strings data.txt | grep =`

This will return our password:

> `FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey`