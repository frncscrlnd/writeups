---
layout: default
title: Level 17 → Level 18
order: 18
---

# Level 17 → Level 18
After logging in with the private key ([Level 16 → Level 17]({{ site.baseurl }}/bandit/level-16_to_level-17) for reference) we can see, by running `ls`, that there are 2 files in the home/bandit17 directory:
`passwords.new` and `passwords.old`. the game tells us that the password for the next level is the only line that has been changed from `passwords.old` to `passwords.new`.

To visualiza the differences between two files we ca use the `diff` command:

`diff passswords.new passwords.old`

This will be the output:

```
bandit17@bandit:~$ diff passwords.new passwords.old
42c42
< x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO
---
> gvE89l3AhAhg3Mi9G2990zGnn42c8v20
```

This means that on line 42 this line

`x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO`

was replaced with this one, which is our new password:

> `gvE89l3AhAhg3Mi9G2990zGnn42c8v20`