---
title: "HackerRank 'Compare Triplets' Solution"
date: "2016-07-29"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "rust"
---

##### Short Problem Definition:

Alice and Bob each created one problem for HackerRank. A reviewer rates the two challenges, awarding points on a scale from 1 to 100  for three categories: _problem clarity_, _originality_, and _difficulty_.

##### Link

[Compare Triplets](https://www.hackerrank.com/challenges/compare-the-triplets)

##### Complexity:

time complexity is O(1)

space complexity is O(1)

##### Execution:

This is a warmup. Follow specification.

##### Solution:

```rust

use std::io;
use std::cmp::Ordering;

fn get_numbers() -> Vec<u32> {
    let mut line = String::new();
    io::stdin().read_line(&mut line).ok().expect("Failed to read line");
    line.split_whitespace().map(|s| s.parse::<u32>().unwrap()).collect()
}

fn main() {
    let a = get_numbers();
    let b = get_numbers();
    
    let mut alice = 0;
    let mut bob = 0;
    
    for idx in 0..3 {
        match a[idx].cmp(&b[idx]) {
            Ordering::Less      => bob += 1,
            Ordering::Greater   => alice += 1,
            Ordering::Equal     => {},
        }
    }
 
    println!("{} {}", alice, bob);
}
```
