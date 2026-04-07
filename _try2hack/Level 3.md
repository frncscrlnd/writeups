---
layout: default
title: try2hack Level 3
description: Writeup of try2hack level 3, a CTF-style challenge about web hacking
order: 3
---

# [Level 3](https://try2hack.nl/levels/level3-.xhtml)

This time the challenges prompts us directly for a password. Looking in the source code we can see:

```
<script type="text/javascript">
    <!--
    pwd = prompt("Please enter the password for level 3:","");
    if (pwd==PASSWORD){
    alert("Allright!\nEntering Level 4 ...");
    location.href = CORRECTSITE;
    }
    else {
    alert("WRONG!\nBack to disneyland !!!");
    location.href = WRONGSITE;
    }
    PASSWORD="AbCdE";
    CORRECTSITE="level4-sfvfxc.xhtml";
    WRONGSITE="http://www.disney.com";
    //-->
</script>
```

however, putting the `AbCdE` password in the prompt will still redirect us to www.disney.com. Going to the specified url https://try2hack.nl/levels/level4-sfvfxc.xhtml will redirect us to a fake Level 4:

```
NOT LEVEL 4
It isn't this easy. Try again :) 
```

We can easily spot what's going on by analyzing the source code closely:

The `if (pwd==PASSWORD)` condition and the `PASSWORD="AbCdE";` assignment happen  at two different times: this means that `PASSWORD` at the time of comparison to `pwd` has a different value.

We can discover the value of `PASSSWORD` just by tiping `PASSWORD` in the console. This will return `try2hackrawks`. Put it in the prompt to get to Level 4.