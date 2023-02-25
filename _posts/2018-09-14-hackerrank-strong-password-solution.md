---
title: "HackerRank 'Strong Password' Solution"
date: "2018-09-14"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

Louise joined a social networking site to stay in touch with her friends. The signup page required her to input a _name_ and a _password_. However, the password must be _strong_. The website considers a password to be _strong_ if it satisfies the following criteria:

- Its length is at least 6.
- It contains at least one digit.
- It contains at least one lowercase English character.
- It contains at least one uppercase English character.
- It contains at least one special character. The special characters are: `!@#$%^&*()-+`

She typed a random string of length n in the password field but wasn't sure if it was strong. Given the string she typed, can you find the minimum number of characters she must add to make her password strong?

##### Link

[Strong Password](https://www.hackerrank.com/challenges/reduced-string)

##### Complexity:

time complexity is O(N)

space complexity is O(1)

##### Execution:

This is a pythonesque solution by [Jay Mistry](https://www.hackerrank.com/jay_mistry456?hr_r=1). The O(4\*N) complexity might be suboptimal, but it showcases a nice use of the language.

##### Solution:

```python

def minimumNumber(n, password):
    count = 0    
    if any(i.isdigit() for i in password)==False:
        count+=1
    if any(i.islower() for i in password)==False:
        count+=1
    if any(i.isupper() for i in password)==False:
        count+=1
    if any(i in '!@#$%^&*()-+' for i in password)==False:
        count+=1
    return max(count,6-n)
```
