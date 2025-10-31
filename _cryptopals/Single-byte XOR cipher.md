---
layout: default
title: Single-byte XOR cipher
set: 1
order: 3
---

# [Single-byte XOR cipher](https://cryptopals.com/sets/1/challenges/3)

To solve this challenge we need to find out which chracter has been used in the XOR operation that gave us this string: 

```
 The hex encoded string:

1b37373331363f78151b7f2b783431333d78397828372d363c78373e783a393b3736

... has been XOR'd against a single character. Find the key, decrypt the message.

You can do this by hand. But don't: write code to do it for you.

How? Devise some method for "scoring" a piece of English plaintext. Character frequency is a good metric. Evaluate each output and choose the one with the best score.

Achievement Unlocked

You now have our permission to make "ETAOIN SHRDLU" jokes on Twitter.
```

The XOR operation ($\oplus$) we know from [Challenge 2 of this set]({{ site.baseurl }}/cryptopals/Fixed-XOR/) is symmetric: the same key is used fo encryption and decryption.

$$
C =\text{ Ciphered text, }P =\text{ Plain text, }K =\text{ Key}
C = P \oplus K
P = C \oplus K
$$

 To find the key we'll use a brtuteforce approach: trying all possible combinations until we get it right. But how do we do that? As the instruction tell us, we need to rate each XORed string by how similar it is to the English language:

```
def score_text(text):
    freq_chars = "ETAOIN SHRDLU"
    return sum(ch in freq_chars for ch in text)
```

This will count the number of characters in the original string that also appear in the freq_chars string, a combination of the characters that are used the most in the English language (learn more about character frequency [here](https://en.wikipedia.org/wiki/Letter_frequency)).

```
best_score = 0
best_key = None
best_plaintext = None
```

Let's set the variables that we'll use later to 0.

```
for key in range(256):
    xored = bytes([b ^ key for b in cipher_bytes])
    plaintext = xored.decode("ascii")

    score = score_text(plaintext)
    if score > best_score:
        best_score = score
        best_key = key
        best_plaintext = plaintext
```

In this for loop the actual XOR operation will take place. The plaintext is then rated and each time the score gets higher the new key and the new plaintext will be saved.

In our case, the XOR key will be the letter `x` (ASCII code `120`) and the plaintext will be `cOOKINGmcSLIKEAPOUNDOFBACON`.

Full code:

```
cipher_hex = "1b37373331363f78151b7f2b783431333d78397828372d363c78373e783a393b3736"
cipher_bytes = bytes.fromhex(cipher_hex)

def score_text(text):
    freq_chars = "ETAOIN SHRDLU"
    return sum(ch in freq_chars for ch in text)

best_score = 0
best_key = None
best_plaintext = None

for key in range(256):
    xored = bytes([b ^ key for b in cipher_bytes])
    plaintext = xored.decode("ascii", errors="ignore")

    score = score_text(plaintext)
    if score > best_score:
        best_score = score
        best_key = key
        best_plaintext = plaintext

print(f"Found key: {best_key} ('{chr(best_key)}')")
print("Deciphered text:", best_plaintext)
with open("plain.txt", "w", encoding="utf-8") as f:
    f.write(best_plaintext)
print("Plaintext saved to plain.txt")
```
