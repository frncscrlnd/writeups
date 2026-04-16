---
layout: default
title: Hack The Box Crocodile
description: Walkthorugh/Writeup for Crocodile, a Starting Point Hack The Box machine about MariaDB, an open-source DBMS
set: Starting Point
subset: Tier 1 - Fundamental Exploitation
order: 3
---

# [Crocodile](https://app.hackthebox.com/machines/Crocodile)<!-- omit in toc -->

This Hack The Box machine mainly revolves around FTP, Gobuster (thus directory enumeration) and nmap.

### Table of contents:
- [Task 1](#task-1)
- [Task 2](#task-2)
- [Task 3](#task-3)
- [Task 4](#task-4)
- [Task 5](#task-5)
- [Task 6](#task-6)
- [Task 7](#task-7)
- [Task 8](#task-8)
- [Task 9](#task-9)
- [Submit Flag](#submit-flag)

## Task 1

*What Nmap scanning switch employs the use of default scripts during a scan?*

As we saw in the [Sequel](https://frncscrlnd.github.io/writeups/hackthebox/Sequel) machine, we can use the

>`-sC`

flag to turn on the use of defaullt scripts.

## Task 2

*What service version is found to be running on port 21?*

Running `nmap target-ip-here -sC` will answer this question, as it will return:

```
PORT   STATE SERVICE
21/tcp open  ftp
| ftp-syst: 
|   STAT: 
| FTP server status:
|      Connected to ::ffff:10.10.14.42
|      Logged in as ftp
|      TYPE: ASCII
|      No session bandwidth limit
|      Session timeout in seconds is 300
|      Control connection is plain text
|      Data connections will be plain text
|      At session startup, client count was 3
|      vsFTPd 3.0.3 - secure, fast, stable
|_End of status
| ftp-anon: Anonymous FTP login allowed (FTP code 230)
| -rw-r--r--    1 ftp      ftp            33 Jun 08  2021 allowed.userlist
|_-rw-r--r--    1 ftp      ftp            62 Apr 20  2021 allowed.userlist.passwd
80/tcp open  http
|_http-title: Smash - Bootstrap Business Template
``` 

This means that 

>`vsFTPd 3.0.3`

is running on port 21

## Task 3

*What FTP code is returned to us for the "Anonymous FTP login allowed" message?*

To login [anonymously](https://learn.microsoft.com/en-us/iis/configuration/system.applicationhost/sites/site/ftpserver/security/authentication/anonymousauthentication) into a FTP instance you'll need to run 

```
ftp anonymous@target-ip-here
```

This will return

```
220 (vsFTPd 3.0.3)
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
```

This means that the FTP code returned for the "Anonymous FTP login allowed" message is

>`230`

## Task 4

*After connecting to the FTP server using the ftp client, what username do we provide when prompted to log in anonymously?*

As we saw in the last task, to login anonymously into a FTP instance you need to use the

>`anonymous`

username.

## Task 5

*After connecting to the FTP server anonymously, what command can we use to download the files we find on the FTP server?*

To [download a file via FTP](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/ftp-get) we need to use the 

>`get`

command. However, we first need to know which files there are. To lits files, use `ls` just like you would on a machine running a GNU/Linux distro

## Task 6

*What is one of the higher-privilege sounding usernames in 'allowed.userlist' that we download from the FTP server?*

We need to download `allowed.userlist` with `get allowed.userlist`. This will return:

```
local: allowed.userlist remote: allowed.userlist
229 Entering Extended Passive Mode (|||49049|)
150 Opening BINARY mode data connection for allowed.userlist (33 bytes).
100% |**********************************************************************************************************************************************************************************************|    33       14.27 KiB/s    00:00 ETA
226 Transfer complete.
33 bytes received in 00:00 (0.15 KiB/s)
```

You can exit the ftp session with `exit`

`allowed.userlist` will be downloaded in your current directory. Reas it with `cat allowed.userlist` :

```
aron
pwnmeow
egotisticalsw
admin
```

The higher-privilege sounding username is definitely

>`admin`

## Task 7

*What version of Apache HTTP Server is running on the target host?*

The nmap scan in [Task 2](#task-2) told us that port 80 is open, but did not tell us which version of Apache HTTP server is running on it. Run `nmap target-ip-here -sV -p 80` to know. This will return

```
Host is up (0.12s latency).

PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
```

That means that the version of Apache running on port 80 is

>`Apache httpd 2.4.41`

## Task 8

*What switch can we use with Gobuster to specify we are looking for specific filetypes?*

We already met the subdomain/directory enumeration tool [Gobuster](https://github.com/OJ/gobuster) in the [Appointment](https://frncscrlnd.github.io/writeups/hackthebox/Appointment) machine. In that challenge we learded that the `dir` flag is needed to tell gobuster we are enumerating directories and not subdomains. Now we need to tell the tool that we want to enumerate directories that end with specific file extensions (`.php`, `.html`, `.pdf`...). In order to do so, according to the [manual](https://github.com/OJ/gobuster#:~:text=%23%20With%20file%20extensions%20gobuster%20dir%20%2Du%20https%3A%2F%2Fexample%2Ecom%20%2Dw%20wordlist%2Etxt%20%2Dx%20php%2Chtml%2Cjs%2Ctxt), to use file xtansions we need the 

>`-x`

flag

## Task 9

*Which PHP file can we identify with directory brute force that will provide the opportunity to authenticate to the web service?*

SInce we know that we are interested in `.php` files, we need to specify this in gobuster:

```
gobuster dir -u target-ip-here -w /usr/share/seclists/Discovery/Web-Content/raft-medium-directories.txt -x php
```

Note that i'm using [seclists](https://github.com/danielmiessler/SecLists) but you can use any wordlist you want. Wordlists are available in `/usr/share/wordlists`

This will return

```
===============================================================
Gobuster v3.8.2
by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
===============================================================
[+] Url:                     http://target-ip-here
[+] Method:                  GET
[+] Threads:                 10
[+] Wordlist:                /usr/share/seclists/Discovery/Web-Content/raft-medium-directories.txt
[+] Negative Status codes:   404
[+] User Agent:              gobuster/3.8.2
[+] Extensions:              php
[+] Timeout:                 10s
===============================================================
Starting gobuster in directory enumeration mode
===============================================================
js                   (Status: 301) [Size: 309] [--> http://target-ip-here/js/]
css                  (Status: 301) [Size: 310] [--> http://target-ip-here/css/]
logout.php           (Status: 302) [Size: 0] [--> login.php]
login.php            (Status: 200) [Size: 1577]
config.php           (Status: 200) [Size: 0]
assets               (Status: 301) [Size: 313] [--> http://target-ip-here/assets/]
fonts                (Status: 301) [Size: 312] [--> http://target-ip-here/fonts/]
dashboard            (Status: 301) [Size: 316] [--> http://target-ip-here/dashboard/]
```

## Submit Flag

*Submit root flag*

We can get from FTP enough info to log into the `login.php` page. Let's see what's the password for `admin` by logging back into an ftp session:

`ftp anonymous@target-ip-here`, then `get allowed.userlist.passwd`. Exit ftp again with `exit`, then `cat allowed.userlist.passwd`. This will return

```
root
Supersecretpassword1
@BaASD&9032123sADS
rKXM59ESxesUFHAd
```

This means that the password fotr the `admin` account is `rKXM59ESxesUFHAd`. Let's try it on the `login.php` page. This will return the `/dashboard/index.php` page, which will give you the flag:

>c7110277ac44d78b6a9fff2232434d16