---
title: "HackerRank 'Diagonal Difference' Solution"
date: "2015-06-15"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
  - "rust"
---

##### Short Problem Definition:

You are given a square matrix of size N×N. Calculate the absolute difference of the sums across the two main diagonals.

Given two strings (they can be of same or different length) help her in finding out the minimum number of character deletions required to make two strings anagrams. Any characters can be deleted from any of the strings.

##### Link

[Diagonal Difference](https://www.hackerrank.com/challenges/diagonal-difference)

##### Complexity:

time complexity is `O(N)`

space complexity is `O(1)`

##### Execution:

Access the correct indexes and compute the difference.

##### Solution:

```python

#!/usr/bin/py

def getDiagonalDifference(v):
    diff = 0
    v_len = len(v)
    for idx in xrange(v_len):
        diff += v[idx][idx]
        diff -= v[idx][v_len-idx-1]

    return abs(diff)    
        
if __name__ == '__main__':
    t = input()
    v = []
    for _ in xrange(t):
        e = map(int, raw_input().split())
        v.append(e)
        
    print getDiagonalDifference(v)
```

* * *

```rust

use std::io;

fn get_number() -> u32 {
    let mut line = String::new();
    io::stdin().read_line(&mut line).ok().expect("Failed to read line");
    line.trim().parse::<u32>().unwrap()
}

fn get_numbers() -> Vec<i32> {
    let mut line = String::new();
    io::stdin().read_line(&mut line).ok().expect("Failed to read line");
    line.split_whitespace().map(|s| s.parse::<i32>().unwrap()).collect()
}

fn main() {
    let siz = get_number() as usize;
    
    let mut left = 0;
    let mut right = 0; 
    
    for i in 0..siz {
        let item = get_numbers();
        left += item[i];
        right += item[siz-i-1];
    } 
    
    println!("{}", (left-right).abs());
   
}
```
