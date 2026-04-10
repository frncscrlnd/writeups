---
layout: default
title: Hack The Box Meow
description: Walkthrough/writeup for your first Hack The Box machine, Meow. 
set: Starting Point
subset: Tier 0 - Foundations
order: 1
---

# [Meow](https://app.hackthebox.com/machines/Meow)<!-- omit in toc -->
This is the first challenge in the Starting Point section of Hack The Box. Since this is probably your first ever Hack The Box challenge, we'll go through every step together.

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

*What does the acronym VM stand for?*

Here the game is referencing the technology that makes this challenge possible: [virtualization](https://en.wikipedia.org/wiki/Virtualization). Behind every Hack The Box machine there is either a **virtual machine** or a [container](https://en.wikipedia.org/wiki/Containerization_(computing)), an isolated environment running its own operating system and services, configured specifically for the challenge. Then the answer for this question is 

>virtual machine

## Task 2

*What tool do we use to interact with the operating system in order to issue commands via the command line, such as the one to start our VPN connection? It's also known as a console or shell.*

This question refers to the [command line interface](https://en.wikipedia.org/wiki/Command-line_interface) of your machine. It's on of the ways you can submit instructions to your computer. It is also usally called a [shell](en.wikipedia.org/wiki/Shell_(computing)) or a

>terminal

## Task 3

*What service do we use to form our VPN connection into HTB labs?*

Here we are being asked about the technology that allows us to reack and attack target Hack The Box machines. just like we saw in te guide, we'll be using 

>OpenVPN

to connect to the machines by running `sudo openvpn your-openvpn-file-here.ovpn`in the terminal.

## Task 4

*What tool do we use to test our connection to the target with an ICMP echo request?*

How do we know whether our machine is connected or not to the target machine? We can use an [ICMP](https://en.wikipedia.org/wiki/Internet_Control_Message_Protocol) echo request, also known as

>ping

to test our connection like this `ping target-ip-here`

## Task 5

*What is the name of the most common tool for finding open ports on a target?*

We are getting closer to the real stuff: according to the [Cyber Kill Chain](https://en.wikipedia.org/wiki/Cyber_kill_chain) the first step of any cyberattack (and therefore security test) is [Reconnaisance](https://en.wikipedia.org/wiki/Footprinting). [Network enumeration](https://en.wikipedia.org/wiki/Network_enumeration) plays one of the biggest roles in reconnaisance as it allows us to list most network services running on a remote machine. The most famous tool for network enumeration is

>nmap

we'll use it by running `nmap target-ip-here`

## Task 6

*What service do we identify on port 23/tcp during our scans?*

nmap will return:

```
Host is up (0.10s latency).
Not shown: 999 closed tcp ports (reset)
PORT   STATE SERVICE
23/tcp open  telnet

Nmap done: 1 IP address (1 host up) scanned in 2.66 seconds
```

This means that port 23 is open and running

>telnet

a remote administration tool, on its [default port](https://en.wikipedia.org/wiki/Telnet).

## Task 7

*What username is able to log into the target over telnet with a blank password?*

telnet's login syntax is straightforward: `telnet target-machine-ip-here 23`

The login process now requires an username and a password:

```
Meow login: 
Password: 
```

The challenge tells us there's an username that requires no password to log in. We'll try some default usernames: 

```
Meow login: admin
Password: 
login incorrect
```

```
Meow login: root
Password: 
Welcome to Ubuntu 20.04.2 LTS (GNU/Linux 5.4.0-77-generic x86_64)
```

we got in, that means the username is

>root

## Submit flag

*Submit root flag*

The challenge now asks us to find a flag. We can list all files and directories in a [GNU/Linux](https://en.wikipedia.org/wiki/Linux) distro with [`ls`](https://www.man7.org/linux/man-pages/man1/ls.1.html). This will return

```
flag.txt  snap
```

we now have to read `flag.txt`. we can do that with [`cat`](https://man7.org/linux/man-pages/man1/cat.1.html) just like this: `cat flag.txt`. This will return:

>b40abdfe23665f766f9c61ecba8a4c19