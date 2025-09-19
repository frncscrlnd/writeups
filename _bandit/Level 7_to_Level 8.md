---
layout: default
title: Level 8
order: 8
---

# Level 7 → Level 8
After logging in with 

`ssh bandit7@bandit.labs.overthewire.org -p 2220`

Password: `morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj`

By using `ls` we can see that in the /bandit7 directory there's a `data.txt` file. Running `cat` on this file returns a wall of text. We can use grep to navigate through this text as we know e have to search for the word "millionth":

`grep millionth data.txt`

This will print out the password:

> `dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc`