---
title: "HackerRank 'Coffee Break Puzzle at Cisco: String Generation' Solution"
date: "2020-07-30"
categories: 
  - "cisco-icode"
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

Watson and Sherlock are two leading researchers in Security and Cryptography at Cisco. They love to solve puzzles related to strings, number theory, encryptions and mapping in their free time. One fine day, during the coffee break, Watson comes up with a puzzle for Sherlock.

_You know my powers, my dear Watson, and yet at the end of three months I was forced to confess that I had at last met an antagonist who was my intellectual equal._

...

##### Link

[Coffee Break Puzzle at Cisco: String Generation](https://www.hackerrank.com/contests/cisco-icode/challenges/sherlock-and-string-generation/problem)

Alternative name: Sherlock and String Generation

##### Complexity:

time complexity is O(N)

space complexity is O(N)

##### Execution:

The rules are explained pretty clearly. There are three rules  
1) each element has to occur at least once  
2) odd elements occur odd number of times, even elements occur even times  
3) the string has to be of length N

Let us compact those rules into examples:  
1+2) mean that each element has to occur at least 1 or 2 times. The minimal string (a suffix really) is "abbcddeff" etc.  
2+3) because of the length limitation, the string can only be the length of the minimal viable suffix (explained above) + a multiply of 2. You could add 2xA, 2xB etc making it "abbcddeffaa" for example. But not "abbcddeffa" because that would violate rule 2).  
extra) the string has to be the minimal lexical variant of all the above rules. That means that you want to be pre-pending A's to your string. Aka the previous one becomes "aaabbcddeff".

##### Solution:

```python
def transform(x):
    return chr(x+96)

def solve(N,K):
    word = []
    for i in xrange(1,K+1):
        if i%2 == 0:
            occurence = 2
        else:
            occurence = 1
        word.extend([i]*occurence)
    
    if (len(word) > N):
        return "No such string."
    
    remaining = N - len(word)
    if remaining%2:
        return "No such string."

    prefix = [1]*remaining
    prefix.extend(word)
    
    return ''.join(map(transform, prefix))
```
