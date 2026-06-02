---
layout: default
title: SadServers "Saint John": what is writing to this log file?
description: Walkthorugh/Writeup for the "Saint John: what is writing to this log file?" challenge, part of the easy section of the SadServers challenges.
set: Easy
order: 1
---

Welcome to this writeup series about [SadServers](https://sadservers.com/) challenges. We'll walk through some interesting Linux and DevOps troubleshooting scenarios.

# ["Saint John": what is writing to this log file?](https://sadservers.com/scenario/saint-john)

This challenge wants us to terminate a running process. Howevers, we first need to know which program is running and writing to `/var/log/bad.log` to terminate it. By using `ls` we can see that there's a `badlog.py` file in our current directory. To find out wheter this is the file that is writing logs to the file, we need to check if it is running or not. We can do so by using `ps aux`. this will return all processes (`a` will return all processes with a terminal, `u` stands for user-oriented format and `x` lists all processes that don't have a terminal).

Among all these processes, one stands out as it's exactly what er are looking for:

`admin        586  0.0  1.7  12508  8228 ?        R    15:20   0:00 /usr/bin/python3 /home/admin/badlog.py`

Yours will have a different PID ([Process ID](https://en.wikipedia.org/wiki/Process_identifier)) since it changes on each startup.

In my case, all i need to do is running `kill 586` and the challenge will be solved. You just need to change PID.