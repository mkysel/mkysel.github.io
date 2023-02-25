---
title: "HackerRank 'Kangaroo' Solution"
date: "2016-08-03"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "rust"
tags: 
  - "timed"
---

##### Short Problem Definition:

There are two kangaroos on an x-axis ready to jump in the positive direction (i.e, toward positive infinity). The first kangaroo starts at location x1 and moves at a rate of v1 meters per jump. The second kangaroo starts at location x2 and moves at a rate of v2 meters per jump. Given the starting locations and movement rates for each kangaroo, can you determine if they'll ever land at the same location at the same time?

##### Alternate Names

Number Line Jumps

##### Link

[Kangaroo](https://www.hackerrank.com/challenges/kangaroo)

##### Complexity:

time complexity is O(1)

space complexity is O(1)

##### Execution:

There is no need to simulate the movement. We can reason that the two kangaroos either meat at the smallest common multiply or never.

##### Solution:

```python
#!/bin/python3

import sys
bl = False
x1,v1,x2,v2 = input().strip().split(' ')
x1,v1,x2,v2 = [int(x1),int(v1),int(x2),int(v2)]
if ((x1>x2 and v1>v2) or (x1<x2 and v1<v2)):
  print("NO")
else:
  for i in range(9999):
    x1 = x1 + v1
    x2 = x2 + v2
    if x1 == x2 :
      bl = True
      break
      
  if bl == True:
    print("YES")
  else:
    print("NO")
```

```
# RUST
// Enter your code here 

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
    let numbers = get_numbers();
    let x1 = numbers[0];
    let v1 = numbers[1];
    let x2 = numbers[2];
    let v2 = numbers[3];
    
     if x1 == x2 && v1 == v2 {
        println!("YES\n");
    }
    else if x1 == x2 && v1 > v2 {
        println!("NO\n");
    }
    else if x1 <= x2 && v1 <= v2 {
        println!("NO\n");
    }
    else {
        if (x2 - x1) % (v1 - v2) == 0  {
            println!("YES\n");
        }
        else {
            println!("NO\n");
        }
    }
    
}
```
