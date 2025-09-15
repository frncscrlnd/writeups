---
layout: default
title: Level 1 → Level 2
order: 2
---

# Level 1 → Level 2
After logging in with 

`ssh bandit1@bandit.labs.overthewire.org -p 2220`

Password: `ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If`

We'll see that typing the `ls` command lists a "-" file. Since the `-` character is not accepted as an argument for cat, we'll nee dot use the whole filename: `./-`

By typing `cat ./-` we can unlock the password for the next level:

`263JGJPfgU6LtdEvgfWU1XP5yac29mFx`