---
layout: default
title: Level 11 → Level 12
order: 12
---

# Level 11 → Level 12
After logging in with 

`ssh bandit11@bandit.labs.overthewire.org -p 2220`

Password: `dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr`

and running `ls` we can see that there is a data.txt file inside the /home/bandit11 directory. As the game tells us, this text file is ROT13 encoded, a type of encoding based on the [Ceasar cipher](https://en.wikipedia.org/wiki/Caesar_cipher) that has a key value of 13. 
We'll need to use `tr` to decode this string. 

`tr` basically replaces characters in a text file by mapping characters from one charset to another (e.g. `tr 'a-z' 'A-Z'`turns the text to uppercase).

In our case: 

`tr 'A-Za-z' 'N-ZA-Mn-za-m' < data.txt`

Where 'A-Za-z' is the first set of chars, 'N-ZA-Mn-za-m' is the second and data.txt is the source file.

this will print our password:

> `7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4`
