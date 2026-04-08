---
layout: default
title: Hack The Box Dancing
description: Walkthorugh/Writeup for Dancing, a Starting Point Hack The Box machine about the SMB protocol
---

# [Dancing](https://app.hackthebox.com/machines/Dancing)<!-- omit in toc -->

This Starting Point Hack The Box machine about the SMB protocol walks us through the basics of Server Message Block

### Table of contents:
- [Task 1](#task-1)
- [Task 2](#task-2)
- [Task 3](#task-3)
- [Task 4](#task-4)
- [Task 5](#task-5)
- [Task 6](#task-6)
- [Task 7](#task-7)
- [Submit flag](#submit-flag)

## Task 1

*What does the 3-letter acronym SMB stand for?*

[SMB](https://en.wikipedia.org/wiki/Server_Message_Block) is a network-wide file sharing protocol. The acronym stands for  

>Server Message Block

## Task 2

*What port does SMB use to operate at?*

Server Message Block runs by defualt at [port([]](https://en.wikipedia.org/wiki/List_of_TCP_and_UDP_port_numbers)

>445

## Task 3

*What is the service name for port 445 that came up in our Nmap scan?*

`nmap target-ip-here` returns

```
Host is up (0.11s latency).
Not shown: 996 closed tcp ports (reset)
PORT     STATE SERVICE
135/tcp  open  msrpc
139/tcp  open  netbios-ssn
445/tcp  open  microsoft-ds
5985/tcp open  wsman

Nmap done: 1 IP address (1 host up) scanned in 5.00 seconds
```

so 

>microsoft-ds

is showing up on port 445

## Task 4

*What is the 'flag' or 'switch' that we can use with the smbclient utility to 'list' the available SMB shares on Dancing?*

The challenge is now telling us to interact with the SMB server. We can do so by using the [`smbclient`](https://www.samba.org/samba/docs/current/man-html/smbclient.1.html) like this: `smbclient target-ip-here command`. With this interface we can list all available SMB shares with the 

>-L

flag/switch 

## Task 5

*How many shares are there on Dancing?*

To determine the number of shares we need to list them with `smbclient //target-ip-here -L` as we saw before. We'll be prompted to enter a password, but we can leave the field blank, as the server is misconfigured. This will return:

```
Password for [WORKGROUP\frncscrlnd]:

        Sharename       Type      Comment
        ---------       ----      -------
        ADMIN$          Disk      Remote Admin
        C$              Disk      Default share
        IPC$            IPC       Remote IPC
        WorkShares      Disk      
Reconnecting with SMB1 for workgroup listing.
```

That means that there are 

>4

shares on this machine.

## Task 6

*What is the name of the share we are able to access in the end with a blank password?*

To answer this question we need to try logging in to every share. We can do this by using `smbclient //target-ip-here/share-name-here`. That means:

```
smbclient //target-ip-here/ADMIN$
Password for [WORKGROUP\fra]:
tree connect failed: NT_STATUS_ACCESS_DENIED
```

that means it's not `ADMIN$`

```
smbclient //target-ip-here/C$
Password for [WORKGROUP\fra]:
tree connect failed: NT_STATUS_ACCESS_DENIED
```

not `C$`

```
smbclient //target-ip-here/IPC$
Password for [WORKGROUP\fra]:
Try "help" to get a list of possible commands.
smb: \> 
```

`C$` works, but we need to check all of them. Type `exit` to try the last one:

```
smbclient //target-ip-here/WorkShares
Password for [WORKGROUP\fra]:
Try "help" to get a list of possible commands.
smb: \> 
```

`WorkShares` works too. Since the answer is in the \*\*\*\*\*\*\*\*\*S format,

>WorkShares

is the answer.

## Task 7

*What is the command we can use within the SMB shell to download the files we find?*

Just like we did in [Fawn](https://frncscrlnd.github.io/writeups/hackthebox/Fawn), we now need to find a way to download files though SMB. The answer is exactly the same:

>get

is the command we can use to download files via SMB.

## Submit flag

*Submit root flag*

Now we need to get a flag. `ls` will list files and directories. There are two directories in the current one:

```
  .                                   D        0  Mon Mar 29 10:22:01 2021
  ..                                  D        0  Mon Mar 29 10:22:01 2021
  Amy.J                               D        0  Mon Mar 29 11:08:24 2021
  James.P                             D        0  Thu Jun  3 10:38:03 2021
```

(`.` and `..` are the current directory and the previous one respectively). Let's `cd` into each one of them and then `ls`:

```
smb: \> cd Amy.J
smb: \Amy.J\> ls
  .                                   D        0  Mon Mar 29 11:08:24 2021
  ..                                  D        0  Mon Mar 29 11:08:24 2021
  worknotes.txt                       A       94  Fri Mar 26 12:00:37 2021

                5114111 blocks of size 4096. 1753652 blocks available
```

nothing inside `Amy.J`

```
smb: \Amy.J\> cd ..
smb: \> cd James.P
smb: \James.P\> ls
  .                                   D        0  Thu Jun  3 10:38:03 2021
  ..                                  D        0  Thu Jun  3 10:38:03 2021
  flag.txt                            A       32  Mon Mar 29 11:26:57 2021

                5114111 blocks of size 4096. 1753652 blocks available
```

there it is: let's read it with `cat flag.txt`:

>035db21c881520061c53e0536e44f815