---
title: "HackerRank 'Save the Prisoner!' Solution"
date: "2016-08-02"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "rust"
tags: 
  - "timed"
---

##### Short Problem Definition:

A jail has N prisoners, and each prisoner has a unique id number,S , ranging from 1 to N. There are M sweets that must be distributed to the prisoners. But wait—there's a catch—the very last sweet S is poisoned! Can you find and print the ID number of the last prisoner to receive a sweet so he can be warned?

##### Link

[Save the Prisoner!](https://www.hackerrank.com/challenges/save-the-prisoner)

##### Complexity:

time complexity is O(1)

space complexity is O(1)

##### Execution:

This challenge is painlessly trivial.

##### Solution:

```java
# RUST

use std::io;

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

fn solve_prisoner(n: u32, m: u32, s: u32) -> u32 {
    ((s - 1 + m - 1 ) % n) +1
}

fn main() {
    let t = get_number();
    for _ in 0..t {
        let line = get_numbers();
        println!("{}", solve_prisoner(line[0], line[1], line[2]));
    }
}
```
