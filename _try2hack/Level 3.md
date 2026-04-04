---
layout: default
title: Level 3
order: 3
---

# [Level 3](https://try2hack.nl/levels/level3-.xhtml)

This time we are prompted with a password before we can even look at the source code. Closing the popup will redirect us to http://www.disney.com. 
There are, however, multiple ways around this. I choose to redirect traffic to [burp](https://portswigger.net/burp), an [HTTP proxy](https://www.fortinet.com/resources/cyberglossary/http-proxy). You can do this too by opening up burpsuite, going over to Proxy, then Intercept (turn "Intercept" to "On") and go to "proxy settings". Here you need to scroll down to Response Interception Rules and turn "Intercept responses based on the following rules:" and the "URL is in target scope" rule on. Then click on Open browser (you can, alternatively, [set up burp as your browser's proxy](https://aatharvauti.github.io/burpsuite/x.%20Foxy%20Proxy.html)) and paste the challenge's URL in the address bar. 

You will intercept both the request and the response respectively to and from this website. After the request has been intercepted, press "Forward". You'll now see the intercepted response. Inside the page's source code, you can see how the login popup works inside some `<script>` tags:

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

This reveals our password, which is `AbCdE` and the next level's URL: https://try2hack.nl/levels/level4-sfvfxc.xhtml