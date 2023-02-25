---
title: "HackerRank 'Bigger is Greater' Solution"
date: "2016-05-13"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
tags: 
  - "timed"
---

##### Short Problem Definition:

Given a word w, rearrange the letters of w to construct another word in such a way that is lexicographically greater than w. In case of multiple possible answers, find the lexicographically smallest one among them.

##### Link

[Bigger is Greater](https://www.hackerrank.com/challenges/bigger-is-greater)

##### Complexity:

time complexity is O(N)

space complexity is O(N)

##### Execution:

This task challenges us to find the next permutation of any given array. There are many implementations available online and it is worthwhile comparing them . I would recommend reading the article by [Nayuki](https://www.nayuki.io/page/next-lexicographical-permutation-algorithm) or re-implementing the [std::next\_permutation](http://en.cppreference.com/w/cpp/algorithm/next_permutation).

##### Solution:

```python

def next_permutation(arr):
    # Find non-increasing suffix
    i = len(arr) - 1
    while i > 0 and arr[i - 1] >= arr[i]:
        i -= 1
    if i <= 0:
        return False
    
    # Find successor to pivot
    j = len(arr) - 1
    while arr[j] <= arr[i - 1]:
        j -= 1
    arr[i - 1], arr[j] = arr[j], arr[i - 1]
    
    # Reverse suffix
    arr[i : ] = arr[len(arr) - 1 : i - 1 : -1]
    return True 
        
def main():
    t = input()
    for _ in xrange(t):
        s = list(raw_input())
        if next_permutation(s):
            print "".join(s)
        else:
            print "no answer"
    
if __name__ == '__main__':
    main()
```
