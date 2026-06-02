---
layout: default
title: SadServers "Saskatoon": counting IPs
description: Walkthorugh/Writeup for the "Saskatoon: counting IPs" challenge, part of the easy section of the SadServers challenges.
set: Easy
order: 2
---

# ["Saskatoon": counting IPs](https://sadservers.com/scenario/saskatoon)

In this challenge we are required to count which IP is making the highest amount of requests to our server and type the answer in `/home/admin/highestip.txt`. In order to do so we have to get to know the log file's structure with `cat /home/admin/access.log`. It will return something like this:

```
100.43.83.137 - - [20/May/2015:21:05:01 +0000] "GET /blog/tags/standards HTTP/1.1" 200 13358 "-" "Mozilla/5.0 (compatible; YandexBot/3.0; +http://yandex.com/bots)"
63.140.98.80 - - [20/May/2015:21:05:28 +0000] "GET /blog/tags/puppet?flav=rss20 HTTP/1.1" 200 14872 "http://www.semicomplete.com/blog/tags/puppet?flav=rss20" "Tiny Tiny RSS/1.11 (http://tt-rss.org/)"
63.140.98.80 - - [20/May/2015:21:05:50 +0000] "GET /blog/geekery/solving-good-or-bad-problems.html?utm_source=feedburner&utm_medium=feed&utm_campaign=Feed%3A+semicomplete%2Fmain+%28semicomplete.com+-+Jordan+Sissel%29 HTTP/1.1" 200 10756 "-" "Tiny Tiny RSS/1.11 (http://tt-rss.org/)"
```

Let's split what we have to do in different tasks:

- we first need to extract all [IPv4 addresses](https://en.wikipedia.org/wiki/IPv4) from the original file. We'll do so with [grep](https://www.man7.org/linux/man-pages/man1/grep.1.html) like this:

`grep -oE '([0-9]{1,3}\.){3}[0-9]{1,3}' access.log`

- however, this is not enough, as it will return just a list of IPs. We now need to find the number of times an IP makes an appearance in the list. In order to do so, we first need to group logs based on the IP that has triad to login. We can do this with [`sort`](https://man7.org/linux/man-pages/man1/sort.1.html):

`grep -oE '([0-9]{1,3}\.){3}[0-9]{1,3}' access.log | sort`

- now we need to count matching adjacent IPs. [`uniq`](https://man7.org/linux/man-pages/man1/uniq.1.html) will normally just check if a row is equal to the ones above and below it and, if not, print that row. `uniq -c`, however, will do the same check recursively and print out the number of times the row appears with the content of the row. We can do so with: 

`grep -oE '([0-9]{1,3}\.){3}[0-9]{1,3}' access.log | sort | uniq -c`

This will return something like

```
    6 62.50.6.113
    7 62.77.171.249
    1 62.97.194.235
    8 63.140.98.80
```

We could just take the IP with the highest value manually, but let's see if we manage to do it all in a command [pipe](https://en.wikipedia.org/wiki/Pipeline_(Unix)).

- we now need to sort the values in descending order. Let's do this with `sort` again, like this:

`grep -oE '([0-9]{1,3}\.){3}[0-9]{1,3}' access.log | sort | uniq -c | sort -nr`

`sort -nr` will sort numerically (`-n`) (numbers like "10" will normally be considered by `sort` as the digit "1", since it's the starting character; however, with `-n` these will be treated as numbers and not digits) and put in reverse (descending) order (`-r`). 

- then we need to select the first entry. We can do so with `head -1` ([`head`](https://man7.org/linux/man-pages/man1/head.1.html) will normally print the first 10 lines of the file, but by specifying `-1` as the argument it will return just the first line.
  
`grep -oE '([0-9]{1,3}\.){3}[0-9]{1,3}' access.log | sort | uniq -c | sort -nr | head -1`

It will return this:

`    482 66.249.73.135`

- however we only need the IP address, so we'll use `grep` again:

`grep -oE '([0-9]{1,3}\.){3}[0-9]{1,3}' access.log | sort | uniq -c | sort -nr | head -1 | grep -oE '([0-9]{1,3}\.){3}[0-9]{1,3}'`

This will return:

`66.249.73.135`

- now to put it into a file we just need the `>` operator to write the IP to a file:

`grep -oE '([0-9]{1,3}\.){3}[0-9]{1,3}' access.log | sort | uniq -c | sort -nr | head -1 | grep -oE '([0-9]{1,3}\.){3}[0-9]{1,3}' > /home/admin/highestip.txt`