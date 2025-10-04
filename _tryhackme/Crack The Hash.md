---
layout: default
title: Crack The Hash
---

This room is all about finding the [hashing function](https://en.wikipedia.org/wiki/Hash_function) ([MD5](https://en.wikipedia.org/wiki/MD5), [SHA-256](https://en.wikipedia.org/wiki/SHA-2), [SHA-1](https://en.wikipedia.org/wiki/SHA-1)...) from a given digest. 
We'll use [hashcat](https://hashcat.net/hashcat/) and [hashcat's wiki examples](https://hashcat.net/wiki/doku.php?id=example_hashes).

### Table of contents:
- [Level 1](#level-1)
- [Level 2](#level-2)

## Level 1

- `48bb6e862e54f2a795ffc4e541caed4d`

First write the digest inside a file `echo 48bb6e862e54f2a795ffc4e541caed4d > hash.txt`

To figure out which hashing function has been used, run 

`hashcat 48bb6e862e54f2a795ffc4e541caed4d` or`hashcat hash.txt`

we'll get something like:

| ID   | Name | Category  |
|------|------|-----------|
| 900  | MD4  | Raw Hash  |
| 0    | MD5  | Raw Hash  |

The first result will be `MD4`, let's try it (`MD4`'s hascat code is `900`):

`hashcat -m 900 -a 0 hash.txt /usr/share/wordlists/rockyou.txt`

`-m` specifies te hashing function, `-a` means it' a [dictionary attack](https://en.wikipedia.org/wiki/Dictionary_attack) and `/usr/share/wordlists/rockyou.txt` is the path to [rockyou](https://en.wikipedia.org/wiki/RockYou), a famous wordlist we are going to use.

`MD4` is wrong, so let's try the second result, `MD5`(code `0`):

`hashcat -m 0 -a 0 hash.txt /usr/share/wordlists/rockyou.txt`

This will return `48bb6e862e54f2a795ffc4e541caed4d:easy`

The plaintext will be:

> `easy`

- `CBFDAC6008F9CAB4083784CBD1874F76618D2A97`
  
Let's do the same for this digest: `> hash.txt` to empty the `hash.txt` file, then `echo CBFDAC6008F9CAB4083784CBD1874F76618D2A97 hash.txt` 

Let's figure out what hashing function has been used:

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

- `1C8BFE8F801D79745C4631D09FFF36C82AA37FC4CCE4FC946683D7B336B63032`

Let's do the same for this digest: `> hash.txt` to empty the `hash.txt` file, then `echo 1C8BFE8F801D79745C4631D09FFF36C82AA37FC4CCE4FC946683D7B336B63032 hash.txt` 

Let's figure out what hashing function has been used:

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

- `$2y$12$Dwt1BZj6pcyc3Dy1FWZ5ieeUznr71EeNkJkUlypTsgbX1H68wsRom`

Let's do it all over again for this digest: `> hash.txt` to empty the `hash.txt` file, then `echo '$2y$12$Dwt1BZj6pcyc3Dy1FWZ5ieeUznr71EeNkJkUlypTsgbX1H68wsRom' hash.txt` 

---
**NOTE**

Use '' quotes as `$` chars won't be displayed as they are used for variables and parameters. 

---

Let's figure out what hashing function has been used:

`haschat $2y$12$Dwt1BZj6pcyc3Dy1FWZ5ieeUznr71EeNkJkUlypTsgbX1H68wsRom` or `hascat hash.txt`

This will return:

`No hash-mode matches the structure of the input hash.`

Hashcat has no clue what function has been used. Let' search [hashcat's wiki examples](https://hashcat.net/wiki/doku.php?id=example_hashes) for this pattern: `$2`

One of the results is [bcrypt](https://en.wikipedia.org/wiki/Bcrypt). The code for this function is `3200`. let's try it:

`hashcat -m 3200 -a 0 hash.txt /usr/share/wordlists/rockyou.txt`

running this command, as thm tells us, will take a long time. This is beacuse bcrypt is a [key derivation function](https://en.wikipedia.org/wiki/Key_derivation_function). What we can do to speed up the process is filtering out combinations of more than 4 chars from `rockyou.txt`:

`LC_ALL=C awk length==4 /usr/share/wordlists/rockyou.txt > ~/rockyou_len4.txt`

or, alternatively, useing grep and regular expressions: 

`LC_ALL=C grep -x .\{4\} /usr/share/wordlists/rockyou.txt > ~/rock4.txt`

- `279412f945939ba78ce0758d3fd83daa`

## Level 2