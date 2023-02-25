---
title: "HackerRank 'Sherlock and The Beast' Solution"
date: "2016-06-21"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
tags: 
  - "timed"
---

##### Short Problem Definition:

Sherlock Holmes suspects his archenemy, Professor Moriarty, is once again plotting something diabolical. Sherlock's companion, Dr. Watson, suggests Moriarty may be responsible for MI6's recent issues with their supercomputer, _The Beast_.

Shortly after resolving to investigate, Sherlock receives a note from Moriarty boasting about infecting _The Beast_with a virus; however, he also gives him a clue—a number, . Sherlock determines the key to removing the virus is to find the largest _Decent Number_ having digits.

##### Link

[Sherlock and The Beast](https://www.hackerrank.com/challenges/sherlock-and-the-beast)

##### Complexity:

time complexity is `O(1)`

space complexity is `O(1)`

##### Execution:

I am presenting two solutions. The naive brute force solution executes in O(N) and passes all the tests. Yet further optimisations and a runtime complexity of O(1) are possible.

When observing the possible output space we realise that there can only be 0, 5 or 10 threes in the output. If there would be 15 threes, it is better to use fives instead. The number of trailing threes can therefore be defined by K = 5\*((2\*N)%3). Let us plug some numbers into the equation:

- 1 -> 5\*(2%3) = 10 -> INVALID
- 2 -> 5\*(4%3) = 5 -> INVALID
- 3 -> 5\*(3%3) = 0 -> 555
- 4 -> 5\*(8%3) = 10 -> INVALID
- 5 -> 5\*(10%3) = 5 -> 33-33-3
- 8 -> 5\*(16%3) = 5 -> 555-33-33-3
- 10 -> 5\*(20%3) = 10 -> 33-33-33-33-33
- 15 -> 5\*(30%3) = 0 -> 555-555-555-555-555

##### Solution:

```python

def sherlockBeast(N):
    K = 5*((2*N)%3)
    if K > N:
        return -1
    else:
        return '5' * (N-K) + '3'*K

if __name__ == '__main__':
    t = input()
    for _ in xrange(t):
        n = input()
        print sherlockBeast(n)
```

```python

def sherlockBeastNaive(N):
    if (N < 3): return "-1" three_cnt = N//3 five_cnt = 0 while three_cnt >=0:
        rem = N - three_cnt*3
        if rem % 5 == 0:
            five_cnt = rem/5
            break
        three_cnt -= 1
        
    if three_cnt <= 0 and five_cnt == 0:
        return "-1"
    
    return "555"*three_cnt + "33333"*five_cnt

if __name__ == '__main__':
    t = input()
    for _ in xrange(t):
        n = input()
        print sherlockBeastNaive(n)
```
