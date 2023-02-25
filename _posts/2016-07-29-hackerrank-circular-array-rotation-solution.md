---
title: "HackerRank 'Circular Array Rotation' Solution"
date: "2016-07-29"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "rust"
---

##### Short Problem Definition:

John Watson performs an operation called a _right circular rotation_ on an array of integers.

##### Link

[Circular Array Rotation](https://www.hackerrank.com/challenges/circular-array-rotation)

##### Complexity:

time complexity is `O(Q)`

space complexity is `O(1)`

##### Execution:

Calculate the offset for every query. Watch out for index overflows and negative modulo.

##### Solution:

```rust

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
 
fn main() {
    let line = get_numbers();
    let (n,k,q) = (line[0], line[1], line[2]);
    let a = get_numbers();
    for _ in 0..q {
        let m = get_number();
        let idx = ((m-(k%n)+n)%n) as usize;
        println!("{}", a[idx]);
    }
}
```
