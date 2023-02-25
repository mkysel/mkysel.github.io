---
title: "HackerRank 'HackerRank Bear and Steady Gene' Solution"
date: "2018-09-20"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
tags: 
  - "timed"
---

##### Short Problem Definition:

A gene is represented as a string of length N (where  is divisible by 4), composed of the letters A, C, G, and T. It is considered to be _steady_ if each of the four letters occurs exactly 1/4 times. For example, GACT and AAGGCCTT are both steady genes.

##### Link

[Bear and Steady Gene](https://www.hackerrank.com/challenges/bear-and-steady-gene)

##### Complexity:

time complexity is O(N)

space complexity is O(1)

##### Execution:

A bit of trivia: I've looked at this problem a couple times 2 years ago and I could not figure it out. Sometimes it is good to take a long break :)

In the first pass I establish how many occurrences there are of each character. Then I establish how many are missing to make the string steady. Remember that the number of occurrences needs to be evenly distributed.

The second pass takes advantage of the caterpillar method. I am looking for the shortest string that contains all the extra characters. They need to be replaced by the missing characters. All others can just be replaced with the same character. The caterpillar extends as long as it does not hit one of the extra characters. It shortens if it contains more extra characters than are needed.

##### Solution:

```python

def steadyGene(gene):
    min_length_string = len(gene)
    
    occurences = dict()
    occurences['A'] = 0
    occurences['G'] = 0
    occurences['C'] = 0
    occurences['T'] = 0
    
    expected = len(gene) // 4
    
    for g in gene:
        occurences[g] += 1
    
    for x in occurences:
        occurences[x] = max(0, occurences[x] - expected)
    
    if occurences['A'] == 0 and occurences['G'] == 0 and occurences['C'] == 0 and occurences['T'] == 0:
        return 0
    
    found = dict()
    found['A'] = 0
    found['G'] = 0
    found['C'] = 0
    found['T'] = 0
    
    tail = 0
    head = 0
    
    while head != len(gene):
        found[gene[head]] += 1
        if found['A'] >= occurences['A'] and \
        found['C'] >= occurences['C'] and \
        found['G'] >= occurences['G'] and \
        found['T'] >= occurences['T']:
            # this is a valid candidate
            min_length_string = min(min_length_string, head-tail+1)
            
            # try to shorten it
            while found[gene[tail]] > occurences[gene[tail]]:
                found[gene[tail]] -= 1
                tail += 1
                min_length_string = min(min_length_string, head-tail+1)
            
            
        head += 1
    
    return min_length_string
```
