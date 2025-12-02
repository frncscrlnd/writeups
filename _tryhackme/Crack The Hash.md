---
layout: default
title: Crack The Hash
---

# [Crack The Hash](https://tryhackme.com/room/crackthehash)

This room is all about finding the [hash function](https://en.wikipedia.org/wiki/Hash_function) ([MD5](https://en.wikipedia.org/wiki/MD5), [SHA-256](https://en.wikipedia.org/wiki/SHA-2), [SHA-1](https://en.wikipedia.org/wiki/SHA-1)...) from a given digest. 
We'll use [hashcat](https://hashcat.net/hashcat/), [hashid](https://www.kali.org/tools/hashid/) and [hashcat's wiki examples](https://hashcat.net/wiki/doku.php?id=example_hashes).

### Table of contents:
- [Crack The Hash](#crack-the-hash)
    - [Table of contents:](#table-of-contents)
  - [Level 1](#level-1)
    - [48bb6e862e54f2a795ffc4e541caed4d](#48bb6e862e54f2a795ffc4e541caed4d)
    - [CBFDAC6008F9CAB4083784CBD1874F76618D2A97](#cbfdac6008f9cab4083784cbd1874f76618d2a97)
    - [1C8BFE8F801D79745C4631D09FFF36C82AA37FC4CCE4FC946683D7B336B63032](#1c8bfe8f801d79745c4631d09fff36c82aa37fc4cce4fc946683d7b336b63032)
    - [$2y$12$Dwt1BZj6pcyc3Dy1FWZ5ieeUznr71EeNkJkUlypTsgbX1H68wsRom](#2y12dwt1bzj6pcyc3dy1fwz5ieeuznr71eenkjkulyptsgbx1h68wsrom)
    - [279412f945939ba78ce0758d3fd83daa](#279412f945939ba78ce0758d3fd83daa)
  - [Level 2](#level-2)
    - [F09EDCB1FCEFC6DFB23DC3505A882655FF77375ED8AA2D1C13F640FCCC2D0C85](#f09edcb1fcefc6dfb23dc3505a882655ff77375ed8aa2d1c13f640fccc2d0c85)
    - [1DFECA0C002AE40B8619ECF94819CC1B](#1dfeca0c002ae40b8619ecf94819cc1b)
    - [$6$aReallyHardSalt$6WKUTqzq.UQQmrm0p/T7MPpMbGNnzXPMAXi4bJMl9be.cfi3/qxIf.hsGpS41BqMhSrHVXgMpdjS6xeKZAs02.](#6areallyhardsalt6wkutqzquqqmrm0pt7mppmbgnnzxpmaxi4bjml9becfi3qxifhsgps41bqmhsrhvxgmpdjs6xekzas02)
    - [e5d8870e5bdd26602cab8dbe07a942c8669e56d6](#e5d8870e5bdd26602cab8dbe07a942c8669e56d6)

## Level 1

### 48bb6e862e54f2a795ffc4e541caed4d

First write the digest inside a file `echo 48bb6e862e54f2a795ffc4e541caed4d > hash.txt`

To figure out which hash function has been used, run 

`hashcat 48bb6e862e54f2a795ffc4e541caed4d` or`hashcat hash.txt`

we'll get something like:

| ID   | Name | Category  |
|------|------|-----------|
| 900  | MD4  | Raw Hash  |
| 0    | MD5  | Raw Hash  |

The first result will be `MD4`, let's try it (`MD4`'s hascat code is `900`):

`hashcat -m 900 -a 0 hash.txt /usr/share/wordlists/rockyou.txt`

`-m` specifies te hash function, `-a` means it' a [dictionary attack](https://en.wikipedia.org/wiki/Dictionary_attack) and `/usr/share/wordlists/rockyou.txt` is the path to [rockyou](https://en.wikipedia.org/wiki/RockYou), a famous wordlist we are going to use.

`MD4` is wrong, so let's try the second result, `MD5`(code `0`):

`hashcat -m 0 -a 0 hash.txt /usr/share/wordlists/rockyou.txt`

This will return `48bb6e862e54f2a795ffc4e541caed4d:easy`

The plaintext will be:

> `easy`

### CBFDAC6008F9CAB4083784CBD1874F76618D2A97
  
Let's do the same for this digest: `> hash.txt` to empty the `hash.txt` file, then `echo CBFDAC6008F9CAB4083784CBD1874F76618D2A97 hash.txt` 

Let's figure out what hash function has been used:

`haschat CBFDAC6008F9CAB4083784CBD1874F76618D2A97` or `hascat hash.txt`

this will return:

| ID   | Name | Category  |
|------|------|-----------|
| 100  | SHA1  | Raw Hash  |

Let's try `SHA1`:

`hashcat -m 100 -a 0 hash.txt /usr/share/wordlists/rockyou.txt`

This will return:

`cbfdac6008f9cab4083784cbd1874f76618d2a97:password123`

the plaintext will be:

> `password123`

### 1C8BFE8F801D79745C4631D09FFF36C82AA37FC4CCE4FC946683D7B336B63032

Let's do the same for this digest: `> hash.txt` to empty the `hash.txt` file, then `echo 1C8BFE8F801D79745C4631D09FFF36C82AA37FC4CCE4FC946683D7B336B63032 hash.txt` 

Let's figure out what hash function has been used:

`haschat 1C8BFE8F801D79745C4631D09FFF36C82AA37FC4CCE4FC946683D7B336B63032` or `hascat hash.txt`

this will return:

| ID   | Name | Category  |
|------|------|-----------|
| 34600  | MD6 (256)  | Raw Hash  |
| 1400  | SHA256  | Raw Hash  |
| 17400  | SHA356  | Raw Hash  |

`MD6` looks like an unpopular function for this type of challenges, so we can skip it. Let's try `SHA256`:

`hashcat -m 1400 -a 0 hash.txt /usr/share/wordlists/rockyou.txt`

This will return:

`1c8bfe8f801d79745c4631d09fff36c82aa37fc4cce4fc946683d7b336b63032:letmein`

so the plaintext will be:

> `letmein`

### $2y$12$Dwt1BZj6pcyc3Dy1FWZ5ieeUznr71EeNkJkUlypTsgbX1H68wsRom

Let's do it all over again for this digest: `> hash.txt` to empty the `hash.txt` file, then `echo '$2y$12$Dwt1BZj6pcyc3Dy1FWZ5ieeUznr71EeNkJkUlypTsgbX1H68wsRom' > hash.txt` 

---
**NOTE**

Use '' quotes as `$` chars won't be displayed as they are used for variables and parameters. 

---

Let's figure out what hash function has been used:

`hashcat '$2y$12$Dwt1BZj6pcyc3Dy1FWZ5ieeUznr71EeNkJkUlypTsgbX1H68wsRom'` or `hashcat hash.txt`

This will return:

| ID     | Name                                                       | Category          |
|-------:|------------------------------------------------------------|-------------------|
| 25600  | bcrypt(md5($pass))                                         | Generic KDF       |
| 25800  | bcrypt(sha1($pass))                                        | Generic KDF       |
| 30600  | bcrypt(sha256($pass))                                      | Generic KDF       |
| 28400  | bcrypt(sha512($pass))                                      | Generic KDF       |
| 3200   | bcrypt $2*$, Blowfish (Unix)                               | Operating System  |

One of the results is [bcrypt](https://en.wikipedia.org/wiki/Bcrypt). The code for this function is `3200`. let's try it:

`hashcat -m 3200 -a 0 hash.txt /usr/share/wordlists/rockyou.txt`

running this command, as thm tells us, will take a long time. This is beacuse bcrypt is a [key derivation function](https://en.wikipedia.org/wiki/Key_derivation_function). What we can do to speed up the process is filtering out combinations of more than 4 chars from `rockyou.txt`:

`LC_ALL=C awk length==4 /usr/share/wordlists/rockyou.txt > ~/rock4.txt`

or, alternatively, useing grep and regular expressions: 

`LC_ALL=C grep -x .\{4\} /usr/share/wordlists/rockyou.txt > ~/rock4.txt`

Running the old command will now take less time:

`hashcat -m 3200 -a 0 hash.txt rock4.txt`

This will return:

`$2y$12$Dwt1BZj6pcyc3Dy1FWZ5ieeUznr71EeNkJkUlypTsgbX1H68wsRom:bleh`

our plaintext will be:

> `bleh`

### 279412f945939ba78ce0758d3fd83daa

New digest, same stuff: `> hash.txt` to empty the `hash.txt` file, then `echo 279412f945939ba78ce0758d3fd83daa > hash.txt`

Let's figure out what hash function has been used:

`hashcat 279412f945939ba78ce0758d3fd83daa` or `hascat hash.txt`

This will return:

| ID   | Name | Category  |
|------|------|-----------|
| 900  | MD4  | Raw Hash  |

Let's try `MD4`:

`hashcat -m 900 -a 0 hash.txt /usr/share/wordlists/rockyou.txt`

this method, unfortunely, won't return anything (`Status...........: Exhausted
`). This means that hashcat ha tried all of rocyou's combinations and they all failed.

What we can still do, however, is checking if this password was already cracked. Let's try [crackstation](https://crackstation.net/):

![crackstation]({{ site.baseurl }}/assets/images/thm/cth/crackstation.png)

We finally got it. Our plaintext is:

> `Eternity22`

## Level 2

### F09EDCB1FCEFC6DFB23DC3505A882655FF77375ED8AA2D1C13F640FCCC2D0C85

First use `> hash.txt` to empty the `hash.txt` file, then write the digest inside a file with `echo F09EDCB1FCEFC6DFB23DC3505A882655FF77375ED8AA2D1C13F640FCCC2D0C85 > hash.txt`

To figure out which hash function has been used, run 

`hashcat F09EDCB1FCEFC6DFB23DC3505A882655FF77375ED8AA2D1C13F640FCCC2D0C85` or`hashcat hash.txt`

we'll get something like:

| ID   | Name | Category  |
|------|------|-----------|
| 34600  | MD6 (256)  | Raw Hash  |
| 1400  | SHA256  | Raw Hash  |

since `MD6` is quite unusual, we'll go ahead with `SHA256`:

`hashcat -m 1400 -a 0 hash.txt /usr/share/wordlists/rockyou.txt`

This will return:

`f09edcb1fcefc6dfb23dc3505a882655ff77375ed8aa2d1c13f640fccc2d0c85:paule`

our plaintext will be:

> `paule`

### 1DFECA0C002AE40B8619ECF94819CC1B

Same as before. First use `> hash.txt` to empty the `hash.txt` file, then write the digest inside a file with `echo 1DFECA0C002AE40B8619ECF94819CC1B > hash.txt`

To figure out which hash function has been used, run 

`hashcat 1DFECA0C002AE40B8619ECF94819CC1B` or`hashcat hash.txt`

we'll get something like:

| ID    | Name                                                       | Category                                |
|-------:|------------------------------------------------------------|-----------------------------------------|
| 900   | MD4                                                        | Raw Hash                                |
| 0     | MD5                                                        | Raw Hash                                |
| 70    | md5(utf16le($pass))                                        | Raw Hash                                |
| 2600  | md5(md5($pass))                                            | Raw Hash salted and/or iterated         |
| 3500  | md5(md5(md5($pass)))                                       | Raw Hash salted and/or iterated         |
| 4400  | md5(sha1($pass))                                           | Raw Hash salted and/or iterated         |
| 20900 | md5(sha1($pass).md5($pass).sha1($pass))                    | Raw Hash salted and/or iterated         |
| 32800 | md5(sha1(md5($pass)))                                      | Raw Hash salted and/or iterated         |
| 4300  | md5(strtoupper(md5($pass)))                                | Raw Hash salted and/or iterated         |
| 1000  | NTLM                                                       | Operating System                        |


As thm tells us, we are going to need to use [`NTLM`](https://en.wikipedia.org/wiki/NTLM), Microsoft's `MD4`-based hash function. `NTLM`'s hashcat code is 1000:

`hashcat -m 1000 -a 0 hash.txt /usr/share/wordlists/rockyou.txt`

This will return:

`1dfeca0c002ae40b8619ecf94819cc1b:n63umy8lkf4i`

So our plaintext will be:

> `n63umy8lkf4i`

### $6$aReallyHardSalt$6WKUTqzq.UQQmrm0p/T7MPpMbGNnzXPMAXi4bJMl9be.cfi3/qxIf.hsGpS41BqMhSrHVXgMpdjS6xeKZAs02.

Just like before, use `> hash.txt` to empty the `hash.txt` file, then write the digest inside a file with `echo $6$aReallyHardSalt$6WKUTqzq.UQQmrm0p/T7MPpMbGNnzXPMAXi4bJMl9be.cfi3/qxIf.hsGpS41BqMhSrHVXgMpdjS6xeKZAs02 > hash.txt`

To figure out which hash function has been used, run 

`hashcat '$6$aReallyHardSalt$6WKUTqzq.UQQmrm0p/T7MPpMbGNnzXPMAXi4bJMl9be.cfi3/qxIf.hsGpS41BqMhSrHVXgMpdjS6xeKZAs02.'` or`hashcat hash.txt`

as i told you for [this digest](#2y12dwt1bzj6pcyc3dy1fwz5ieeuznr71eenkjkulyptsgbx1h68wsrom):

---
**NOTE**

If you type hashcat [digest], remember to use '' quotes as `$` chars won't be displayed as they are used for variables and parameters. 

---

both `hashcat` and `hashid` don't return anything. We can try [crackstation](https://crackstation.net/) again, but it does nothing again.

This means that we have to resort to looking for this digest's pattern among [hashcat's wiki examples](https://hashcat.net/wiki/doku.php?id=example_hashes).

Once we've gone through them, it turns out that the hashing function that's been used is `sha512crypt` the function used in the `/etc/shadow` folder to hash passwords. Its' hashact code is 1800:

`hashcat -a 0 -m 1800 '$6$aReallyHardSalt$6WKUTqzq.UQQmrm0p/T7MPpMbGNnzXPMAXi4bJMl9be.cfi3/qxIf.hsGpS41BqMhSrHVXgMpdjS6xeKZAs02.' /usr/share/wordlists/rockyou.txt`

Since this can take a lot of time, we can filter combinations of more then 6 chars from `rockyou.txt` like we did for [this digest](#2y12dwt1bzj6pcyc3dy1fwz5ieeuznr71eenkjkulyptsgbx1h68wsrom):

`LC_ALL=C awk length==6 /usr/share/wordlists/rockyou.txt > ~/rock6.txt`

then run 

`hashcat -m 1800 -a 0 hash.txt rock6.txt`

we'll get:

`$6$aReallyHardSalt$6WKUTqzq.UQQmrm0p/T7MPpMbGNnzXPMAXi4bJMl9be.cfi3/qxIf.hsGpS41BqMhSrHVXgMpdjS6xeKZAs02.:waka99`

so our plaintext will be:

> `waka99`

### e5d8870e5bdd26602cab8dbe07a942c8669e56d6

Thm tells us the hashing function that has been used is [HMAC-SHA1](https://en.wikipedia.org/wiki/HMAC). This means that the format for our hashcat parameter needs to be `digest:salt`. That means:

`e5d8870e5bdd26602cab8dbe07a942c8669e56d6:tryhackme`

For one last time, use `> hash.txt` to empty the `hash.txt` file, then write the digest and sal inside a file with `echo e5d8870e5bdd26602cab8dbe07a942c8669e56d6:tryhackme > hash.txt`

HMAC-SHA1's hashcat code is 160, so we'll need to run:

`hashcat -a 0 -m 160 hash.txt /usr/share/wordlists/rockyou.txt`

we'll get:

`e5d8870e5bdd26602cab8dbe07a942c8669e56d6:tryhackme:481616481616`

this emans that out plaintext will be:

> `481616481616`

That's it! This room is over.