---
layout: default
title: Fixed XOR
set: 1
order: 2
---

# [Fixed XOR](https://cryptopals.com/sets/1/challenges/2)

In this challenge we need to write a script that decodes two strings from [hex](https://en.wikipedia.org/wiki/Hex_dump) and [XORs](https://en.wikipedia.org/wiki/Exclusive_or) them together.

You can choose your preferred language for this, i chode Python. First we need to convert the strings from hex to raw bytes. `bytes` and `fromhex()` allow us to do just that:

```
a = "1c0111001f010100061a024b53535009181c"
a_bytes = bytes.fromhex(a)
```

It's the same for the other string. Now onto the XOR part: we need to iterate through all of the strings' characters and this means that we need a for loop that will execute for the number of characters the first string has.

```
for i in range(len(a_bytes)):
    x = a_bytes[i]
    y = b_bytes[i]
    result_list.append(x ^ y)
```

This section returns a list that has the result of XOR operation between the two strings. Now we need to turn it back into a hed:

```
result = bytes(result_list)
print(result.hex())
```

This will also print the resulting hex string. 

Full code:

```
a = "1c0111001f010100061a024b53535009181c"
b = "686974207468652062756c6c277320657965"

a_bytes = bytes.fromhex(a)
b_bytes = bytes.fromhex(b)

result_list = []

for i in range(len(a_bytes)):
    x = a_bytes[i]
    y = b_bytes[i]
    result_list.append(x ^ y)

result = bytes(result_list)
print(result.hex())
```
