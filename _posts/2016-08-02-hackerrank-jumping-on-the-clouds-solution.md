---
title: "HackerRank 'Jumping on the Clouds' Solution"
date: "2016-08-02"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "rust"
tags: 
  - "timed"
---

##### Short Problem Definition:

Emma is playing a new mobile game involving clouds numbered from 1 to n. There are two types of clouds, ordinary clouds and thunderclouds. The game ends if Emma jumps onto a thundercloud, but if she reaches the last cloud, she wins the game!

##### Link

[Jumping on the Clouds](https://www.hackerrank.com/challenges/jumping-on-the-clouds)

##### Complexity:

time complexity is O(N)

space complexity is O(N)

##### Execution:

Theoretically your solution can depend on the fact that the win condition is guaranteed, but I don't like such solutions. Here I present a semi-DP approach that keeps track of the optimal number of jumps it takes to reach each cloud.

##### Solution:

```java
# RUST

use std::io;
use std::cmp;

fn get_number() -> u32 {
    let mut line = String::new();
    io::stdin().read_line(&mut line).ok().expect("Failed to read line");
    line.trim().parse::<u32>().unwrap()
}

fn get_numbers() -> Vec<u32> {
    let mut line = String::new();
    io::stdin().read_line(&mut line).ok().expect("Failed to read line");
    line.split_whitespace().map(|s| s.parse::<u32>().unwrap()).collect()
}

fn calculate_jumping(a: Vec<u32>, n: usize) -> u32{
    let mut v = vec![100; n];
    v[0] = 0;
    
    for i in 1..n {
        //println!("{} {} {:?}", i, a[i], v);
        if a[i] == 1 {
            continue;
        }
        
        if i == 1 {
            v[i] = v[i-1] + 1;
        } else {
            v[i] = cmp::min(v[i-1], v[i-2]) + 1;
        }
    }
    
    v[n-1]
}

fn main() {
    let n = get_number() as usize;
    let a = get_numbers();
 
    println!("{}", calculate_jumping(a, n) );
}
```
