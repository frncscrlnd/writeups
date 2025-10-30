---
layout: default
title: Level 16 → Level 17
order: 17
---

# [Level 16 → Level 17](https://overthewire.org/wargames/bandit/bandit17.html)

After logging in with 

`ssh bandit15@bandit.labs.overthewire.org -p 2220`

Password: `kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx`

we can see that there isn't any file in the home/bandit16. this is becuse, as the game tells us, we have to first find on which port (between  31000 to 32000) of the localhost machine the server is listening on, then find which of these active ports supports SSL/TLS, then submit the password for the current level to it via openssh s_client.

First, we have to find open ports on the localhost: nmap will help us. nmap is a port scanning utility that sends TCP/UDP packets to scan for open ports. In our case, we'll need the standard `SYN` scan on ports  31000-32000:

`nmap -p 31000-32000 localhost`

This will return:

```
PORT      STATE SERVICE
31046/tcp open  unknown
31518/tcp open  unknown
31691/tcp open  unknown
31790/tcp open  unknown
31960/tcp open  unknown
```

This means that there are 5 open ports in this port range on localhost. Now we just need to find which one supports ssl. Let's run  nmap -p [port] -sV localhost on each of our ports:

```
nmap -p 31046 -sV localhost
nmap -p 31518 -sV localhost
nmap -p 31691 -sV localhost
nmap -p 31790 -sV localhost
nmap -p 31960 -sV localhost
```

This will return, respectively to each command:

```
PORT      STATE SERVICE VERSION
31046/tcp open  echo
```

```
PORT      STATE SERVICE  VERSION
31518/tcp open  ssl/echo
```

```
PORT      STATE SERVICE VERSION
31691/tcp open  echo
```

```
PORT      STATE SERVICE     VERSION
31790/tcp open  ssl/unknown
```

```
PORT      STATE SERVICE VERSION
31960/tcp open  echo
```

The port we are looking for is claarly 31790, as it supports SSL and has an unknown service (but we definitely don't need 31518's echo service, as it would give us back our password). Let's connect to the 31790 port with:

`openssl s_client -connect localhost:31790`

and send our password. This will return the private key for our next level:

> ``` 
> -----BEGIN RSA PRIVATE KEY-----
> MIIEogIBAAKCAQEAvmOkuifmMg6HL2YPIOjon6iWfbp7c3jx34YkYWqUH57SUdyJ
> imZzeyGC0gtZPGujUSxiJSWI/oTqexh+cAMTSMlOJf7+BrJObArnxd9Y7YT2bRPQ
> Ja6Lzb558YW3FZl87ORiO+rW4LCDCNd2lUvLE/GL2GWyuKN0K5iCd5TbtJzEkQTu
>DSt2mcNn4rhAL+JFr56o4T6z8WWAW18BR6yGrMq7Q/kALHYW3OekePQAzL0VUYbW
> JGTi65CxbCnzc/w4+mqQyvmzpWtMAzJTzAzQxNbkR2MBGySxDLrjg0LWN6sK7wNX
> x0YVztz/zbIkPjfkU1jHS+9EbVNj+D1XFOJuaQIDAQABAoIBABagpxpM1aoLWfvD
> KHcj10nqcoBc4oE11aFYQwik7xfW+24pRNuDE6SFthOar69jp5RlLwD1NhPx3iBl
> J9nOM8OJ0VToum43UOS8YxF8WwhXriYGnc1sskbwpXOUDc9uX4+UESzH22P29ovd
> d8WErY0gPxun8pbJLmxkAtWNhpMvfe0050vk9TL5wqbu9AlbssgTcCXkMQnPw9nC
> YNN6DDP2lbcBrvgT9YCNL6C+ZKufD52yOQ9qOkwFTEQpjtF4uNtJom+asvlpmS8A
> vLY9r60wYSvmZhNqBUrj7lyCtXMIu1kkd4w7F77k+DjHoAXyxcUp1DGL51sOmama
> +TOWWgECgYEA8JtPxP0GRJ+IQkX262jM3dEIkza8ky5moIwUqYdsx0NxHgRRhORT
> 8c8hAuRBb2G82so8vUHk/fur85OEfc9TncnCY2crpoqsghifKLxrLgtT+qDpfZnx
> SatLdt8GfQ85yA7hnWWJ2MxF3NaeSDm75Lsm+tBbAiyc9P2jGRNtMSkCgYEAypHd
> HCctNi/FwjulhttFx/rHYKhLidZDFYeiE/v45bN4yFm8x7R/b0iE7KaszX+Exdvt
> SghaTdcG0Knyw1bpJVyusavPzpaJMjdJ6tcFhVAbAjm7enCIvGCSx+X3l5SiWg0A
> R57hJglezIiVjv3aGwHwvlZvtszK6zV6oXFAu0ECgYAbjo46T4hyP5tJi93V5HDi
> Ttiek7xRVxUl+iU7rWkGAXFpMLFteQEsRr7PJ/lemmEY5eTDAFMLy9FL2m9oQWCg
> R8VdwSk8r9FGLS+9aKcV5PI/WEKlwgXinB3OhYimtiG2Cg5JCqIZFHxD6MjEGOiu
> L8ktHMPvodBwNsSBULpG0QKBgBAplTfC1HOnWiMGOU3KPwYWt0O6CdTkmJOmL8Ni
> blh9elyZ9FsGxsgtRBXRsqXuz7wtsQAgLHxbdLq/ZJQ7YfzOKU4ZxEnabvXnvWkU
> YOdjHdSOoKvDQNWu6ucyLRAWFuISeXw9a/9p7ftpxm0TSgyvmfLF2MIAEwyzRqaM
> 77pBAoGAMmjmIJdjp+Ez8duyn3ieo36yrttF5NSsJLAbxFpdlc1gvtGCWW+9Cq0b
> dxviW8+TFVEBl1O4f7HVm6EpTscdDxU+bCXWkfjuRb7Dy9GOtt9JPsX8MBTakzh3
> vBgsyi/sN3RqRBcGU40fOoZyfAMT8s1m/uYv52O6IgeuZ/ujbjY=
> -----END RSA PRIVATE KEY-----
> ```

Let's connect copy the key to a file: like we did in [Level 12 → Level 13]({{ site.baseurl }}/bandit/level-12_to_level-13), let's `cd` into `/tmp` and make a temporary directory with `mktemp -d`; then move into the new directory with cd and create a file by running `touch sshkey.private` (or whatever name you want to give to the file). Use nano to edit the file by running `nano sshkey.private`, then paste the content of the key. The we need to change the permissions for this file, as a private key can only be used by one user: 

`chmod 700 sshkey.private`

This means that the user has read, write and execute (rwx) permissions, while the group ond other users have no permissions.

Then we can login into the next level with:

> `ssh -p 2220 -i sshkey.private bandit17@localhost`