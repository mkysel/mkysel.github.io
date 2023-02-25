---
title: "HackerRank 'Plus Minus' Solution"
date: "2015-06-16"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
  - "rust"
---

##### Short Problem Definition:

You're given an array containing integer values. You need to print the fraction of count of positive numbers, negative numbers and zeroes to the total numbers. Print the value of the fractions correct to 3 decimal places.

##### Link

[Plus Minus](https://www.hackerrank.com/challenges/plus-minus)

##### Complexity:

time complexity is O(N)

space complexity is O(1)

##### Execution:

Count the values. Do not forget to force floats instead of integers.

##### Solution:

```python

#!/usr/bin/py

def getPartitionPythonesque(values):
    c1 = len(filter(lambda x:x>0,values))
    c2 = len(filter(lambda x:x<0,values))
    c3 = len(filter(lambda x:x==0,values))
    v_len = float(len(values))
    
    return (c1/v_len, c2/v_len, c3/v_len)

def getPartition(values):
    pos, neg, zero = [0.0,0.0,0.0]
    v_len = len(values)
    
    for value in values:
        if value == 0:  zero += 1
        elif value > 0: pos += 1
        else:           neg += 1
            
    return (pos/v_len, neg/v_len, zero/v_len)    
    

if __name__ == '__main__':
    t = input()
    values = map(int, raw_input().split())
    partition = getPartition(values)
    for percentage in partition:
        print round(percentage,4)
```

* * *

```rust

use std::io;
use std::cmp::Ordering;


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
    let siz = get_number() as f32;
    
    let (mut pos, mut neg, mut zer) = (0, 0, 0);
    
    let zero = 0;
    
    for i in get_numbers() {
        match i.cmp(&zero) {
            Ordering::Less => neg+=1,
            Ordering::Greater => pos+=1,
            Ordering::Equal => zer+=1,
        }
    }
       
    println!("{:.6}\n{:.6}\n{:.6}", pos as f32 /siz, neg as f32 /siz, zer as f32 /siz);
}
```
