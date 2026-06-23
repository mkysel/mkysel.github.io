---
title: "HackerRank 'Staircase' Solution"
date: "2015-06-17"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
  - "rust"
---

##### Short Problem Definition:

Your teacher has given you the task to draw the structure of a staircase. Being an expert programmer, you decided to make a program for the same. You are given the height of the staircase.

##### Link

[Staircase](https://www.hackerrank.com/challenges/staircase)

##### Complexity:

time complexity is `O(N^2)`

space complexity is `O(1)`

##### Execution:

Either fill each level with N-i empty spaces or adjust to the right.

##### Solution:

```python

#!/usr/bin/py
def printStaircase(levels):
    for i in xrange(1,levels+1):
        print ("#" * i).rjust(levels)


if __name__ == '__main__':
    t = input()
    printStaircase(t)
```

* * *

```rust

use std::io;

fn get_number() -> u32 {
    let mut line = String::new();
    io::stdin().read_line(&mut line).ok().expect("Failed to read line");
    line.trim().parse::<u32>().unwrap()
}

fn main(){
    let n = get_number() as usize;
    
    for i in 0..n {
        let s = std::iter::repeat("#").take(i+1).collect::<String>();
        println!("{:>width$}", s, width=n);
    }
    
}
```
