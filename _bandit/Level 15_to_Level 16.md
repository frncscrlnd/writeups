---
layout: default
title: Level 15 → Level 16
order: 16
---

# Level 15 → Level 16
After logging in with 

`ssh bandit15@bandit.labs.overthewire.org -p 2220`

Password: `8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo`

we can see, by running, `ls`, that the /home/bandit15 directory is empty. this is beacause, as the game tells us, we need to submit the password for this level to port 30001 on localhost on an cnrypted connection using SSL/TLS. We can do this by using openSSL's `s_client`, which makes our connection secure:

`openssl s_client -connect localhost:30001`

We can then submit our password. This will return the password fo the next level:

> `kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx`