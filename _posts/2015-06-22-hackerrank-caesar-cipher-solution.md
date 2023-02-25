---
title: "HackerRank 'Caesar Cipher' Solution"
date: "2015-06-22"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

Julius Caesar protected his confidential information from his enemies by encrypting it. Caesar rotated every alphabet in the string by a fixed number K. This made the string unreadable by the enemy. You are given a string S and the number K. Encrypt the string and print the encrypted string.

##### Link

[Caesar Cipher](https://www.hackerrank.com/challenges/caesar-cipher-1)

##### Complexity:

time complexity is O(?)

space complexity is O(?)

##### Execution:

This task has a few non-obvious pitfalls.

- upper case does not rotate into lower case (as one would expect from ascii)
- k can be bigger than the alphabet size (and must be corrected or a modulo operation on the whole sum must be performed)

Basically you need separate logic for upper case and lower case letters.

##### Solution:

```python

#!/usr/bin/py
def encryptCaesar(s, k):
    output = list(s)
    k %= (ord('z') - ord('a') + 1)
    
    for idx, l in enumerate(output):
        if l.isalpha():
            if l.isupper():
                new_char = ord(l)+k
                if new_char > ord('Z'):
                    new_char = new_char - ord('Z') + ord('A') - 1
                output[idx] = chr(new_char)
            else:
                new_char = ord(l)+k
                if new_char > ord('z'):
                    new_char = new_char - ord('z') + ord('a') - 1
                output[idx] = chr(new_char)
    return ''.join(output)
    
    
if __name__ == '__main__':
    n = input()
    s = raw_input()
    k = input()
    print encryptCaesar(s, k)
    
```
