---
title: "HackerRank 'Non-Divisible Subset' Solution"
date: "2016-08-05"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
  - "rust"
tags: 
  - "timed"
---

##### Short Problem Definition:

Given a set _S_ of _n_ distinct integers, print the size of a maximal subset _S'_ of _S_ where the sum of any 2 numbers in _S'_ are not evenly divisible by _k_.

##### Link

[Non-Divisible Subset](https://www.hackerrank.com/challenges/non-divisible-subset)

##### Complexity:

time complexity is `O(N)`

space complexity is `O(N)`

##### Execution:

This is by all means not an easy task and is also reflected by the high failure ratio of the participants. For a sum of two numbers to be evenly divisible by k the following condition has to hold. If the remainder of _N1%k == r_ then _N2%k = k-r_ for _N1+N2 % k == 0_. Let us calculate the set of all numbers with a remainder of _r_ and _k-r_ and pick the larger set. If the remainder is half of k such as 2 % 4 = 2 or exactly k such as 4 % 4 = 0, just one number from each of these sets can be contained in _S'._

##### Solution:

```python
def solveSubset(S, k, n):
    r = [0] * k

    for value in S:
        r[value%k] += 1

    result = 0
    for a in xrange(1, (k+1)//2):
        result += max(r[a], r[k-a])
    if k % 2 == 0 and r[k//2]:
        result += 1
    if r[0]:
        result += 1
    return result

n, k = map(int, raw_input().split())
S = map(int, raw_input().split())
print solveSubset(S, k, n)
```

* * *

```java
# RUST

use std::io;

fn get_numbers() -> Vec<u32> {
    let mut line = String::new();
    io::stdin().read_line(&mut line).ok().expect("Failed to read line");
    line.split_whitespace().map(|s| s.parse::<u32>().unwrap()).collect()
}

fn calculate_nondivisible(a: Vec<u32>, n: usize, k: usize) -> u32 {
    let mut result = 0;
    
    let mut r = vec![0; k];
    for val in a {
        r[(val as usize)%k] += 1;
    }
    
    for idx in 1..(k+1)/2 {
        result += std::cmp::max(r[idx as usize], r[(k-idx) as usize]);
    }
    
    if k % 2 == 0 && r[k/2] != 0 {
        result += 1;
    }
    if r[0] != 0 {
        result += 1;
    }
    
    result
}

fn main() {
    let line = get_numbers();
    let a = get_numbers();
 
    println!("{}", calculate_nondivisible(a, line[0] as usize, line[1] as usize) );
}
```
