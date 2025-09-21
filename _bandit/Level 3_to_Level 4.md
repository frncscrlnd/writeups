---
layout: default
title: Level 3 → Level 4
order: 4
---

# Level 3 → Level 4
After logging in with 

`ssh bandit3@bandit.labs.overthewire.org -p 2220`

Password: `MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx`

that the game tells us that the file we're looking for is hidden in the inhere directory.
`ls` tells us we are in the same directory as the `inhere` directory, let's move to id with `cd inhere`.

`ls` tells us nothing now. As we know the file we're looking for is hidden (hidden files start with `.`, e.g. `hidden.md`) we'll have to use `ls -a` to show all files (including hidden ones).

This command tells us there is a `...Hiding-From-You` file in this directory; let's read its' content with `cat ...Hiding-From-You`:

The password is `2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ`