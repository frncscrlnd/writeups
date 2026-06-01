---
layout: default
title: FIRST CTF office 1
description: Walkthorugh/Writeup for the office 1 challenge, part of the Inter-regional CyberDrill October 2023 section of the FIRST CTF.
set: Inter-regional CyberDrill October 2023
order: 5
---

# [office 1](https://ctf.first.org/challenges#office%20file%201-13)

This challenge is a follow up to the last one, [zip 1](https://frncscrlnd.github.io/writeups/first/zip-1). It tells us to [brute-force](https://en.wikipedia.org/wiki/Brute-force_attack) an Microsoft Word file's password. To access the `.docx` file you need to open the `it2.zip` file and insert the password we found earlier.

We now get a `it2.docx` file. Just like we did for [zip 1](https://frncscrlnd.github.io/writeups/first/zip-1), we'll use [`john`](https://www.openwall.com/john/) to turn the file into an hash:

`office2john it2.docx > hash.txt`

This time we won't guess the password (we already have a hint: the password starts with kuld and is 7 characters long) but we'll brute force it with [`hashcat`](https://hashcat.net/hashcat/) starting from the original hint, `kuld***`.

To use hashcat in brute-force mode, we need to use the `-a 3` flag. As we can see from the [example hashes](https://hashcat.net/wiki/doku.php?id=example_hashes) hashcat's code for MS Office 2013 is `9600`. Our command's mask (the unknown part of the string) will be made of 4 characters. We'll use different placeholders for different charsets:

```
?l lowercase (a-z) 
?u uppercase (A-Z)
?d digits (0-9) 
?s symbols 
?a all
```

Let's try lowercase alphabetical characters first:

`hashcat -m 9600 hash.txt -a 3 kuld?l?l?l`

We'll get:

`$office$*2013*100000*256*16*4fa274b82c7479757311707852c1d7de*b84fd3d4dd538a6484089051e92dff4f*95c194e063f98b8739e2115113b9f858449ac5e4c518ff2eca764d69391df8f9:kuldeep`

This means that our password will be:

>kuldeep
