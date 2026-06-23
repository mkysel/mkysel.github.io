---
title: "HackerRank 'The Minion Game' Solution"
date: "2020-07-30"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

Kevin and Stuart want to play the '**The Minion Game**'.

**Game Rules**

Both players are given the same string, S.  
Both players have to make substrings using the letters of the string S.  
Stuart has to make words starting with _consonants_.  
Kevin has to make words starting with _vowels_.  
The game ends when both players have made all possible substrings.

##### Link

[The Minion Game](https://www.hackerrank.com/challenges/the-minion-game/problem)

##### Complexity:

time complexity is `O(N)`

space complexity is `O(1)`

##### Execution:

Now, your brain might go straight to the generation of subsets. You might even start thinking about overlapping subsets. But you already know that each substring at position P starts with the same letter. Hence the solution is to iterate over the list once.

##### Solution:

```python
def minion_game(string):
    vowels_list = set(['a','e','i','o','u','A','E','I','O','U'])
    consonants = 0
    vowels = 0

    n = len(string)
    for i, l in enumerate(string):
        if l in vowels_list:
            vowels += n-i
        else:
            consonants += n-i

    if vowels == consonants:
        print "Draw"
    elif vowels > consonants:
        print "Kevin {}".format(vowels)
    else:
        print "Stuart {}".format(consonants)
```
