---
title: "HackerRank 'Palindrome Index' Solution"
date: "2015-08-29"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

You are given a string of lower case letters. Your task is to figure out the index of the character on whose removal it will make the string a palindrome. There will always be a valid solution.

##### Link

[Palindrome Index](https://www.hackerrank.com/challenges/palindrome-index)

##### Complexity:

time complexity is `O(N)`

space complexity is `O(N)`

##### Execution:

The solution seems n^2 but isPalindrome is executed only once. Due to the problem specification there exists only one valid solution (and it always exists). Therefore I process the string as in a normal palindrome check, if it fails I try to figure out if the removal of the left index helps or not. If it helps the left index is the answer, if it does not, the right index is the answer. It can be further optimised, by not making a copy of the string in the isPalindrome input parameters.

##### Solution:

```python

#!/usr/bin/py

def isPalindrome(s):
    for idx in xrange(len(s)//2):
        if s[idx] != s[len(s)-idx-1]:
            return False
    return True

def palindromeIndex(s):
    for idx in xrange((len(s)+1)//2):
        if s[idx] != s[len(s)-idx-1]:
            if isPalindrome(s[:idx]+s[idx+1:]):
                return idx
            else:
                return len(s)-idx-1
    return -1


def main():
    t = input()
    for _ in xrange(t):
        s = raw_input()
        print palindromeIndex(s)
    
if __name__ == '__main__':
    main()
```
