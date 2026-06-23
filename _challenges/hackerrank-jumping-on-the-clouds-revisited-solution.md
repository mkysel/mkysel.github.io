---
title: "HackerRank 'Jumping on the Clouds: Revisited' Solution"
date: "2016-08-01"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
  - "rust"
tags: 
  - "timed"
---

##### Short Problem Definition:

Aerith is playing a cloud game! In this game, there are clouds numbered sequentially from 1 to n. Each cloud is either an _ordinary cloud_ or a _thundercloud_. Given the values of _n_ and _k_ the configuration of the clouds, can you determine the final value of _e_ after the game ends?

##### Link

[Jumping on the Clouds: Revisited](https://www.hackerrank.com/challenges/jumping-on-the-clouds-revisited)

##### Complexity:

time complexity is `O(N)`

space complexity is `O(1)`

##### Execution:

Simulate the game in a loop.

##### Solution:

```python
#!/bin/python

def solveCloudRevisited(c, n, k):
    pos = 0
    cnt = 0

    while cnt == 0 or pos != 0:
        pos += k
        pos %= n
        if c[pos] == 0:
            cnt += 1
        else:
            cnt += 3
    
    return 100 - cnt

if __name__ == '__main__':
    n,k = map(int,raw_input().strip().split(' '))
    c = map(int,raw_input().strip().split(' '))
    print solveCloudRevisited(c, n, k)
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

fn calculate_jumping(a: Vec<u32>, n: usize, k: usize) -> u32 {
    let mut e = 0;
    let mut pos = 0;
    
    while e == 0 || pos != 0 {
        e += match a[pos] {
            0 => 1,
            1 => 3,
            _ => panic!("invalid input"),
        };
        
        pos += k;
        pos %= n;
    }
    
    100 - e
}

fn main() {
    let line = get_numbers();
    let (n, k) = (line[0], line[1]);
    let a = get_numbers();
    println!("{}", calculate_jumping(a, n as usize, k as usize) );
}
    
```
