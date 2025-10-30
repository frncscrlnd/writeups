---
layout: default
title: Convert hex to base64
set: 1
order: 1
---

"the cryptopals crypto challenges" are 8 sets of 10 cryptography-based challenges. I'll try to cover them all in the following writeups. 

# Convert hex to base64

In this challenge we need to turn an hex string into a base64 encoded string:

```
The string:

49276d206b696c6c696e6720796f757220627261696e206c696b65206120706f69736f6e6f7573206d757368726f6f6d

Should produce:

SSdtIGtpbGxpbmcgeW91ciBicmFpbiBsaWtlIGEgcG9pc29ub3VzIG11c2hyb29t
```

In order to do so, we first need to decode the hex string into ASCII characters. We'll do this with [xxd](https://www.geeksforgeeks.org/linux-unix/xxd-command-in-linux/), a tool that allows both creating a [hexadecimal dump](https://en.wikipedia.org/wiki/Hex_dump) and converting an hexdump back into binary form. 

The correct syntax will be:

`xxd -r -p [hex_file] > [plaintext_file]`

This will return:

`I'm killing your brain like a poisonous mushroom`, a lyric from a [song](https://www.youtube.com/watch?v=rog8ou-ZepE) i wish i had forgotten.

We'll then use [`base64`](https://en.wikipedia.org/wiki/Base64) to encode the string.

`base64 [plaintext_file] > [base64_file]`

This will finally return

`SSdtIGtpbGxpbmcgeW91ciBicmFpbiBsaWtlIGEgcG9pc29ub3VzIG11c2hyb29t`
