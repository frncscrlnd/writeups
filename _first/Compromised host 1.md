---
layout: default
title: FIRST CTF Compromised host 1
description: Walkthorugh/Writeup for the compromised host 1 challenge, part of the network section of the FIRST CTF.
set: Network
order: 1
---

# [Compromised host (1/4)](https://ctf.first.org/challenges#Compromised%20host%20(1/4)-42)

This challenge gives us a `network_traffic.pcap` file to analyze. It also tells us that a [network scan](https://nmap.org/book/man-port-scanning-basics.html) has occurred. It then asks us "What port/ports were opened on the host?". [TCP (Transmission Control Protocol)](en.wikipedia.org/wiki/Transmission_Control_Protocol) is a connection-oriented trnasport level protocol. This means that data exchange happens after a [triple handshake](https://en.wikipedia.org/wiki/Transmission_Control_Protocol#Connection_establishment).

This "triple handshake" is estabilished via specific TCP segments:

- `SYN`
- `SYN-ACK`
- `ACK`

If the server replies with `SYN-ACK` to an initial `SYN`, that means that port is open. Otherwise, the server would have dropped the segment or responded with a `RST`.

This means that we have look for `SYN-ACK` segments in our `.pcap` file.

After opening up `network_traffic.pcap` in [Wireshark](https://www.wireshark.org/), we can use this filter in the filter bar: `tcp.flags.syn==1 and tcp.flags.ack==1`. The resulting segments will be:

![synack]({{ site.baseurl }}/assets/images/first/network/synack.png)

`SYN-ACK` segments directed towards the attacker (`192.168.1.18`) come from the following ports:

>22,2375

This will be the challenge's flag.