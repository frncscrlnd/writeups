---
layout: default
title: FIRST CTF zip 1
description: Walkthorugh/Writeup for the zip 1 challenge, part of the Inter-regional CyberDrill October 2023 section.
set: Inter-regional CyberDrill October 2023
order: 4
---

# [zip 1](https://ctf.first.org/challenges?#zip%201-12)

This challenge tells us to find the password to the `it2.zip` file. After downloading the file, we can use [`zip2john`](https://github.com/openwall/john/blob/bleeding-jumbo/src/zip2john.c) to turn it into an hash file, then use [`john`](https://www.openwall.com/john/) to [guess the password](https://en.wikipedia.org/wiki/Password_cracking) from a wordlist. 

Let's turn the file into a [digest](https://csrc.nist.gov/glossary/term/hash_digest):

```
zip2john it2.zip > hash.txt
```

Then crack the password with:

```
john hash.txt
```

This will return:

![kubicek]({{ site.baseurl }}/assets/images/first/2023/kubicek.png)

This means that the zip file password will be:

>kubicek