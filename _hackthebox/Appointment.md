---
layout: default
title: Hack The Box Appointment
description: Walkthorugh/Writeup for Appointment, a Starting Point Hack The Box machine about SQL
set: Starting Point
subset: Tier 1 - Fundamental Exploitation
order: 1
---

# [Dancing](https://app.hackthebox.com/machines/Dancing)<!-- omit in toc -->

This Starting Point Hack The Box machine revolves around the SQL language.

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
- [Submit Flag](#submit-flag)

## Task 1

*What does the acronym SQL stand for?*

[SQL](https://en.wikipedia.org/wiki/SQL) stands for 

>Structured Query Language

and is used to manage data in relational DBMSs.

## Task 2

*What is one of the most common type of SQL vulnerabilities?*

The most common type of SQL [vulnerability](https://en.wikipedia.org/wiki/Vulnerability_(computer_security)) is 

>SQL injection

or SQLi. It's a SQL-specific [code injection vulnerability](https://en.wikipedia.org/wiki/Code_injection).

## Task 3

*What is the 2021 OWASP Top 10 classification for this vulnerability?*

We can answer this question by visiting the 2021 OWASP Top 10 page](https://owasp.org/Top10/2021/). [OWASP](https://owasp.org/) (Open Worldwide Application Security Project) is a foundation that works to create and mantain application security frameworks such as [WSTG](https://owasp.org/www-project-web-security-testing-guide/v42/). On that page you can scroll down to Injection and see that the code related to SQL injections is

>A03:2021-Injection

## Task 4

*What does Nmap report as the service and version that are running on port 80 of the target?*


We now need to run a [nmap](https://nmap.org/download) scan on the target machine. Port 80 sits inside the [most used 1000 ports](https://nmap.org/book/man-port-specification.html), so we don't need to specify a port. However, we need to know the service and the version that is running on port 80, so we need the `-sV` flag: `nmap target-ip-here -sV` which will return

```
PORT   STATE SERVICE VERSION
80/tcp open  http    Apache httpd 2.4.38 ((Debian))
```

Now we know that

>Apache httpd 2.4.38 ((Debian))

is running on that port

## Task 5

*What is the standard port used for the HTTPS protocol?*

As we've seen before, [port](https://en.wikipedia.org/wiki/Port_(computer_networking)) 80 is usually the standard port for the [HTTP protocol](https://en.wikipedia.org/wiki/HTTP). [HTTPS](https://en.wikipedia.org/wiki/HTTPS), however, is the secure version of HTTP and runs on port 

>443

## Task 6

*What is a folder called in web-application terminology?*

A [folder](https://en.wikipedia.org/wiki/Folder_(computing)) in web-application technology is also known as a

>directory

## Task 7

*What is the HTTP response code that is returned for `Not Found` errors?*

[HTTP response codes](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Status) are quite many and mean different things: 

1. Informational responses (100 – 199)
2. Successful responses (200 – 299)
3. Redirection messages (300 – 399)
4. Client error responses (400 – 499)
5. Server error responses (500 – 599)

`Not Found` errors fall under the `400` range, as the client is to be blamed for an unexisting direcroty being requested. More specifically, you'll see a

>`404`

status code.

## Task 8

*Gobuster is one tool used to brute force directories on a webserver. What switch do we use with Gobuster to specify we're looking to discover directories, and not subdomains?*

To solve this tasl we'll need [Gobuster](https://github.com/OJ/gobuster), which is included in [Kali linux](https://www.kali.org/). We'll basically use a wordlist

## Task 9
## Task 10
## Submit Flag