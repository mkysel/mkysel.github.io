---
title: "HackerRank 'Connecting Towns' Solution"
date: "2020-08-04"
categories: 
  - "coding-challenge"
  - "golang"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

Gandalf is travelling from **Rohan** to **Rivendell** to meet Frodo but there is no direct route from **Rohan** (T1) to **Rivendell** (Tn).

But there are towns T2,T3,T4...Tn-1 such that there are N1 routes from Town T1 to T2, and in general, Ni routes from Ti to Ti+1 for i=1 to n-1 and 0 routes for any other Ti to Tj for j ≠ i+1

Find the total number of routes Gandalf can take to reach Rivendell from Rohan.

##### Link

[Connecting Towns](https://www.hackerrank.com/challenges/connecting-towns/problem)

##### Complexity:

time complexity is `O(N)`

space complexity is `O(1)`

##### Execution:

This is a simple combinatorics problem. Even though python ints don't overflow easily, I added the MOD anyways. The code in GoLang is also available.

##### Solution:

```python
MOD = 1234567

def main():
    t = input()
    for _ in xrange(t):
        n = input()
        arr = map(int, raw_input().split())
        routes = 1
        for value in arr:
            routes = ( routes * value ) % MOD
        print routes
    
if __name__ == '__main__':
    main()
```

* * *

```go
func connectingTowns(n int32, routes []int32) int32 {
    result := int32(1)
    for _, value := range routes {
        result = (result*value) % 1234567
    }
    return result
}
```
