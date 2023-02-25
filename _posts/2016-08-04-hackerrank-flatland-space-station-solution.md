---
title: "HackerRank 'Flatland Space Station' Solution"
date: "2016-08-04"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "rust"
tags: 
  - "timed"
---

##### Short Problem Definition:

For each city, determine its distance to the _nearest_ space station and _print the maximum_ of these distances.

##### Link

[Flatland Space Station](https://www.hackerrank.com/challenges/flatland-space-stations)

##### Complexity:

time complexity is O(N)

space complexity is O(N)

##### Execution:

This is a two pass algorithm. First, measure the distance to the last station on the left. And on the second pass measure the distance to the nearest station on the right. Pick the minimum of both values. Remember that the first and last position are not necessarily stations.

##### Solution:

```java
# RUST

use std::io;
use std::cmp;

fn get_numbers() -> Vec<u32> {
    let mut line = String::new();
    io::stdin().read_line(&mut line).ok().expect("Failed to read line");
    line.split_whitespace().map(|s| s.parse::<u32>().unwrap()).collect()
}

fn find_distance(c: Vec<u32>, n: usize) -> u32 {
    //println!("{:?}", c);
    let mut solution = 0;
    let mut distances = vec![n as u32; n];
    
    let mut last_seen = 0;
    let mut seen_one = false;
    for i in 0..n {
        if c[i] == 1 {
            seen_one = true;
            last_seen = 0;
        } else {
            last_seen += 1;
        }
        if seen_one {
            distances[i] = last_seen;
        }
    }
    //println!("{:?}", distances);
    
    
    let mut last_seen = 0;
    let mut seen_one = false;
    for i in (0..n).rev() {
        if c[i] == 1 {
            seen_one = true;
            last_seen = 0;
        } else {
            last_seen += 1;
        }
        solution = cmp::max(solution, 
                            match seen_one {
                                true => cmp::min(last_seen, distances[i]),
                                false => distances[i],
                            }
                    );
        //println!("{} {} {}", i, last_seen, solution);
    }
    
    solution
}

fn main() {
    let line = get_numbers();
    let n = line[0] as usize;
    let c = get_numbers();
    let mut v = vec![0; n];
    for station in c {
        v[station as usize] = 1;
    }
    println!("{}", find_distance(v, n));
}
    
```
