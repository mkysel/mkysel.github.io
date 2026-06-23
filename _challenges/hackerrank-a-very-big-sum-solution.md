---
title: "HackerRank 'A Very Big Sum' Solution"
date: "2019-05-07"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
  - "rust"
tags: 
  - "timed"
---

##### Short Problem Definition:

Calculate and print the sum of the elements in an array, keeping in mind that some of those integers may be quite large.

##### Link

[A Very Big Sum](https://www.hackerrank.com/challenges/a-very-big-sum/problem)

##### Complexity:

time complexity is `O(N)`

space complexity is `O(1)`

##### Execution:

Just add all of this together. No magic.

##### Solution:

```python
#!/usr/bin/py
if __name__ == '__main__':
    t = input()
    n = map(int, raw_input().split())
    print sum(n)
```

* * *

```cpp
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

fn main() {
    get_number();  
    let sum = get_numbers().iter().fold(0u64, |a, &b| a + b as u64);
    println!("{}", sum)
}
```
