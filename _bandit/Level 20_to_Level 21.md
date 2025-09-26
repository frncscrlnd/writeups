---
layout: default
title: Level 20 → Level 21
order: 21
---

# Level 20 → Level 21
After logging in with 

`ssh bandit20@bandit.labs.overthewire.org -p 2220`

Password: `0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO`

by using `ls` we become aware that ther is a `suconnect` file in the home/bandit20 directory. This is, as the game tells us, a setuid binary that makes a connection to localhost on the port you specify as a commandline argument. It then reads a line of text from the connection and compares it to the password in the previous level (`0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO` from [Level 19 → Level 20]({{ site.baseurl }}/bandit/level-19_to_level-20)). If the password is correct, it will transmit the password for the next level ([Level 20 → Level 21]({{ site.baseurl }}/bandit/level-20_to_level-21)). 
To verify our password we first need to set up a listener where we are going to send the password from, then run ./suconnect [port number] on a second terminal. To do this we must run a netcat listener with nc -lp [port number]. To choose which port number we're going to use (it is important for us not to choose a port number that's already in use) we are gonna run nmap on our machine like we did in [Level 16 → Level 17]({{ site.baseurl }}/bandit/level-16_to_level-17): 

`nmap localhost`

we'll get something along the lines of: 

```
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-09-25 11:58 UTC
Nmap scan report for localhost (127.0.0.1)
Host is up (0.00017s latency).
Not shown: 990 closed tcp ports (conn-refused)
PORT      STATE SERVICE
22/tcp    open  ssh
1111/tcp  open  lmsocialserver
1840/tcp  open  netopia-vo2
4321/tcp  open  rwhois
8000/tcp  open  http-alt
8001/tcp  open  vcom-tunnel
8002/tcp  open  teradataordbms
8080/tcp  open  http-proxy
30000/tcp open  ndmps
50001/tcp open  
```

Let's choose a free port number; i'm going to use port `9999`. Let's start our listener: `nc -lp 9999`. 

Let's login on a new terminal like we did back at the start of this writeup.
Let's also run `./suconnect 9999` in this second terminal.

Let's submit the password on the `nc -lp 9999` terminal. On our `./suconnect 9999` terminal we'll get:
```
Read: 0qXahG8ZjOVMN9Ghs7iOWsCfZyXOUbYO
Password matches, sending next password
```

On our `nc -lp 9999` terminal we'll get our password:

> `EeoULMCra2q0dSkYj561DX7s1CpBuOBt`