---
layout: default
title: Level 2 → Level 3
order: 3
---

# [Level 2 → Level 3](https://overthewire.org/wargames/bandit/bandit3.html)
After logging in with 

`ssh bandit2@bandit.labs.overthewire.org -p 2220`

Password: `263JGJPfgU6LtdEvgfWU1XP5yac29mFx`

that the `ls` command shows a `--spaces in this filename--` file. Since we know from [Level 2]({{ site.baseurl }}/bandit/level-1_to_level-2), every file whose name contains a `-` must be passed as an argument only by its' full filename:

`cat ./--spaces in this filename--` should do the job then, right?

Well not really, as there also are some spaces inside the filename. This means we'll have to wrap the filename in quotes:

cat `./"--spaces in this filename--"` wil give us our password:

> `MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx`