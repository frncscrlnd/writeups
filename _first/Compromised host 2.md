---
layout: default
title: FIRST CTF Compromised host 2
description: Walkthorugh/Writeup for the compromised host 2 challenge, part of the network section of the FIRST CTF.
set: Network
order: 2
---

# [Compromised host (2/4)](https://ctf.first.org/challenges?#Compromised%20host%20(2/4)-43)

This challenge tells us that `network_traffic.pcap` captured traffic coming from a compromised [Docker](https://docs.docker.com/get-started/docker-overview/) [container](https://en.wikipedia.org/wiki/Containerization_(computing)).

The best way to inspect such traffic is using a `http` filter on [Wireshark](https://www.wireshark.org/). The resulting packets will be:

![http]({{ site.baseurl }}/assets/images/first/network/http.png)

The packet we are looking for also comes from the server and is directed to the attacker. Let's add filters for source and address IPs like this: `http && ip.src == 192.168.1.25 && ip.dst == 192.168.1.18`

![srcdst]({{ site.baseurl }}/assets/images/first/network/srcdst.png)

Let's look at the specific packet that has this request URI: `/v1.42/exec/29e16f99002956474a3f86bf38b17822192876e11ac26d6b9d7adc5e8f2fe4ce/json`. This is an HTTP response that holds data about a specific container. If we look through the JavaScript Object Notation (JSON) content, we'll see a `Container ID` key with its value:

`62d1f694c5d71e79f65003ce80bc2132c03cabd839c0cea62489f93dd9dc87b1`

The challenge only requires us to paste the first 12 characters from the ID, so the flag will be:

>62d1f694c5d7