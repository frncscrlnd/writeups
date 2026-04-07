---
layout: default
title: try2hack Level 2
description: Writeup of try2hack level 2, a CTF-style challenge about web hacking
order: 2
---

# [Level 2](https://try2hack.nl/levels/level2-xfdgnh.xhtml)

Level 2 is a bit trickier due to [Adobe Flash being gone](https://en.wikipedia.org/wiki/Adobe_Flash_Player): if you open this level in a regular browser it will most certainly look incomplete as the [.swf] file that is embedded in the page will not show properly:

```
<param name="movie" value="level2.swf">
```

does, in fact, show nothing. 

Despite Flash being gone, the challenge is still solvable:

We can install [ffdec](https://github.com/jindrapetrik/jpexs-decompiler) and decompile the `.swf` file to extract the next level's URL.

After installing the decompiler and opening `level2.swf` we can finally see what the login script looks like by looking at the `BUTTONCONDACTION` field:

![ffdec]({{ site.baseurl }}/assets/images/t2h/2/2.2.png)

This tells us that the form's username is `try2hack` while the password is `irtehh4x0r!`

Also, most importantly the next level's url is `level3-.xhtml`

We could simply append this to the website's URL like this https://try2hack.nl/levels/level3-.xhtml but we can also render it as intended by installing the [ruffle Flash emulator](https://ruffle.rs/) extension on our browser: 

![login]({{ site.baseurl }}/assets/images/t2h/2/2.1.png)
