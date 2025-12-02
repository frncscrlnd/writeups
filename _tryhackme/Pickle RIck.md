---
layout: default
title: Pickle Rick
---

# [Pickle Rick](https://tryhackme.com/room/picklerick)

This Rick and Morty-themed challenge requires you to exploit a web server and find three ingredients to help Rick make his potion and transform himself back into a human from a pickle.

### Table of contents:
- [Pickle Rick](#pickle-rick)
    - [Table of contents:](#table-of-contents)
  - [What is the first ingredient that Rick needs?](#what-is-the-first-ingredient-that-rick-needs)
  - [What is the second ingredient in Rick’s potion?](#what-is-the-second-ingredient-in-ricks-potion)
  - [What is the last and final ingredient?](#what-is-the-last-and-final-ingredient)

## What is the first ingredient that Rick needs?

We are given a web application's IP address, which from now on will be called `[target_IP]`. Since we know it's a web application, we expect this address to be reachable through a browser. Let's first check which services are running on this host using [nmap](nmap.org):

`nmap [target_IP]`

This will return

```
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http
```

This means that ssh is potentially vulnerable to a [password-guessing attack](https://attack.mitre.org/techniques/T1110/001/). But we first need to get a username. 

Open up `[target_IP]` in a browser. You'll be welcomed by this page:

![homepage]({{ site.baseurl }}/assets/images/thm/pr/home.png)

Navigating through the html code, you'll stumble upon this comment:

![homecomment]({{ site.baseurl }}/assets/images/thm/pr/homecomment.png)

Since `R1ckRul3s` could well be an SSH username, we might as well try and use [hydra](https://www.kali.org/tools/hydra/):

`hydra -l R1ckRul3s -P /usr/share/wordlists/rockyou.txt [target_IP] ssh`

This will return

`[ERROR] target ssh://[target_IP]:22/ does not support password authentication (method reply 4)`

Meaning that this is not the right way.

Since the web application's homepage provides no additional information, let's esplore more of the app by enumerating its' directories by using [dirb](https://www.kali.org/tools/dirb/):

`dirb http://[target_IP] /usr/share/wordlists/dirb/common.txt -X .php,.txt,.html,.js`

The `-X` flag means extensions. All words in the wordlist will be followed by these extensions.

This will return

```
+ http://[target_IP]/denied.php (CODE:302|SIZE:0)                                                                                                                    
+ http://[target_IP]/index.html (CODE:200|SIZE:1062)                                                                                                                 
+ http://[target_IP]/login.php (CODE:200|SIZE:882)                                                                                                                   
+ http://[target_IP]/portal.php (CODE:302|SIZE:0)                                                                                                                    
+ http://[target_IP]/robots.txt (CODE:200|SIZE:17)     
```

We now have a new page to investigate: `login.php` (`portal.php` redirects you to this page too).

![login]({{ site.baseurl }}/assets/images/thm/pr/login.png)

We now have a use for our `R1ckRul3s` username. Let's see if we can find the password with what we've got:

opening up `http://[target_IP]/robots.txt` in a browser reveals a weird text: 

`Wubbalubbadubdub`

which could well be our password. Let's try logging in to `http://[target_IP]/login.php`:

![Rick Portal]({{ site.baseurl }}/assets/images/thm/pr/portal.png)

We now need to find the first ingredient. If Command panel is really what it's name suggests, let's try enumerating files on this machine:

`ls`

This will return these files:

```
Sup3rS3cretPickl3Ingred.txt
assets
clue.txt
denied.php
index.html
login.php
portal.php
robots.txt
```

Let's try and read the content of `Sup3rS3cretPickl3Ingred.txt`:

`cat Sup3rS3cretPickl3Ingred.txt`

This will return, however, an image:

![disabled command]({{ site.baseurl }}/assets/images/thm/pr/disabled.png)

This probably means that the cat command is disabled. But don't lose hope: we can use plenty of commands to read this .txt file: 

`tac Sup3rS3cretPickl3Ingred.txt`

This will return:

> `mr. meeseek hair`

Which is the first ingredient.
 
## What is the second ingredient in Rick’s potion?
## What is the last and final ingredient?