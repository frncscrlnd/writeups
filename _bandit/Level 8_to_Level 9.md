---
layout: default
title: Level 8 → Level 9
order: 9
---

# Level 8 → Level 9
After logging in with 

`ssh bandit8@bandit.labs.overthewire.org -p 2220`

Password: `dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc`

By using `ls` we can see that in the /bandit8 directory there's a `data.txt` file. Using `cat` on this file returns a wall of text. The game tells us we haave to search for the only line of text tha occurs only once. We can try using `uniq` with the `-u` flag(`uniq .u data.txt`), but this returns all of the WoT again. This happens beacuse `uniq -u` only works on adjacent lines of text, so it compares one line to the next one, but repeated lines can be on  lines. We can solve this by using `sort`: this command sorts the lines by the position of each line's characters on the ASCII table. It will return equal lines in adjacent positions, making it possible for `uniq -u` to find the unique one:

> `4CKMh1JI91bUIZZPXDqGanal4xvAg0JM`