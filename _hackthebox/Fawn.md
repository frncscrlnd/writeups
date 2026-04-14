---
layout: default
title: Hack The Box Fawn
description: Walkthrough/writeup of Fawn, a Tsarting Point Hack The Box machine about FTP
set: Starting Point
subset: Tier 0 - Foundations
order: 2
---

# [Fawn](https://app.hackthebox.com/machines/Fawn)<!-- omit in toc -->

This Starting Point machine revolves entirely around [FTP](https://en.wikipedia.org/wiki/File_Transfer_Protocol), a protocol designed for sharing files across a network. 

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
- [Task 10](#task-10)
- [Task 11](#task-11)
- [Submit flag](#submit-flag)

## Task 1

*What does the 3-letter acronym FTP stand for?*

As we said, we are talking about 

>File Transfer Protocol

## Task 2

*Which port does the FTP service listen on usually?*

This protocol usually runs on [port](https://en.wikipedia.org/wiki/Port_(computer_networking)#Port_number)

>21

## Task 3

*FTP sends data in the clear, without any encryption. What acronym is used for a later protocol designed to provide similar functionality to FTP but securely, as an extension of the SSH protocol?*

Most of the time, secure versions of protocols only add a final `s` to the original acronym. This is also true this time.

>FTPS

## Task 4

*What is the command we can use to send an ICMP echo request to test our connection to the target?*

Just like we did in [Meow](https://frncscrlnd.github.io/writeups/hackthebox/Meow), one of the ways to test our connection to another machine is through the

>ping

command.

## Task 5

*From your scans, what version is FTP running on the target?*

`nmap target-id-here -sV` (`-sV` means "service version") will return

```
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
Service Info: OS: Unix
```

this means that the ftp version is

>vsftpd 3.0.3

## Task 6

*From your scans, what OS type is running on the target?*

Again, from our last scan, the machine is running

>Unix

## Task 7

*What is the command we need to run in order to display the 'ftp' client help menu?*

Most programs will need something like `-h` or `--help`, but client applications, such as ftp, need `-?`, so

>ftp -?

## Task 8

*What is username that is used over FTP when you want to log in without having an account?*

Opposite to what we did in [Meow](https://frncscrlnd.github.io/writeups/hackthebox/Meow), there are no default credentials to try other than the standard one:

>anonymous

## Task 9

*What is the response code we get for the FTP message 'Login successful'?*

ftp login syntax is simply `ftp anonymous@target-ip-here (port if not default 21)`. This will return

```
220 (vsFTPd 3.0.3)
331 Please specify the password.
Password: 
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> 
```

we are now logged in and the success code is

>230

## Task 10

*There are a couple of commands we can use to list the files and directories available on the FTP server. One is dir. What is the other that is a common way to list files on a Linux system.*

Just like we did in [Meow](https://frncscrlnd.github.io/writeups/hackthebox/Meow), 

>ls

will list files and directories from the specifued directory (`.`, the current directory, as default)

## Task 11

*What is the command used to download the file we found on the FTP server?*

To [download a file from ftp](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/ftp-get) we'll need 

>get

## Submit Flag

*Submit root flag*

`ls` will show:

```
150 Here comes the directory listing.
-rw-r--r--    1 0        0              32 Jun 04  2021 flag.txt
226 Directory send OK.
```

we can donload `flag.txt` to our machine with `get flag.txt`

```
ftp> get flag.txt
local: flag.txt remote: flag.txt
229 Entering Extended Passive Mode (|||20834|)
150 Opening BINARY mode data connection for flag.txt (32 bytes).
100% |**********************************************************************************************************************************************************************************************|    32      512.29 KiB/s    00:00 ETA
226 Transfer complete.
32 bytes received in 00:00 (0.74 KiB/s)
```

now we can exit ftp with `quit` and read `flag.txt` just like we did in [Meow](https://frncscrlnd.github.io/writeups/hackthebox/Meow) with `cat flag.txt`. This will return:

>035db21c881520061c53e0536e44f815