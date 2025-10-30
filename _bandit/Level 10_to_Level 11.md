---
layout: default
title: Level 10 → Level 11
order: 11
---

# [Level 10 → Level 11](https://overthewire.org/wargames/bandit/bandit11.html)
After logging in with 

`ssh bandit10@bandit.labs.overthewire.org -p 2220`

Password: `FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey`

and running `ls` we can see that there is a data.txt file inside the /home/bandit10 directory. as the game tells us, the data inside this file is base 64 encoded. 

To decode from base64, simply use `base64 -d data.txt` (`-d` stands for decode). This will give us our password:

> `dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr`