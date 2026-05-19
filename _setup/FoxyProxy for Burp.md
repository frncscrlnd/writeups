---
layout: default
title: FoxyProxy for Burp
description: How to set up FoxyProxy to use Burp in your own browser. 
---

# [FoxyProxy](https://getfoxyproxy.org/) for [Burp](https://portswigger.net/burp/communitydownload)

In this tutorial we'll explore how to configure FoxyProxy tu use Burp in your browser. FoxyProxy is a browser extension available for [most browsers](https://getfoxyproxy.org/downloads/#:~:text=Chrome,Safari) that allows users to manage proxies from their browser. 

In our specific case, FoxyProxy will allow us to use Burp as a proxy to the requests made by our browser, thus making it possible for us to run security tests from the comfort of our own browser.

Start by adding FoxyProxy Standard (on Firefox) or FoxyProxy Basic (on Chrome) to your browser. Once installed, click on it and open the `Options` menu:

![options]({{ site.baseurl }}/assets/images/setup/options.png)

Then click on `Add`:

![add]({{ site.baseurl }}/assets/images/setup/add.png)

And fill these fields like this:

![fields]({{ site.baseurl }}/assets/images/setup/fields.png)

Then click again on the extension, select `Burp`, open `burpsuite` and open whatever web page you want. If requests are being intercepted by Burp, it works. 