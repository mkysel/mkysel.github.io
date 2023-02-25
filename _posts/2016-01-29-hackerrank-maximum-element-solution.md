---
title: "HackerRank 'Maximum Element' Solution"
date: "2016-01-29"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

You have an empty sequence, and you will be given N queries. Each query is one of these three types:

1. Push the element x into the stack.
2. Delete the element present at the top of the stack.
3. Print the maximum element in the stack.

##### Link

[Maximum Element](https://www.hackerrank.com/challenges/maximum-element)

##### Complexity:

time complexity is O(N)

space complexity is O(N)

##### Execution:

I really enjoyed this problem. I did not see the solution at first, but after it popped up, it was really simple.

Keep two stacks. One for the actual values and one (non-strictly) increasing stack for keeping the maxima.

##### Solution:

```python


class CustomStack:
    def __init__(self):
        self.stack = []
        self.maxima = []
    
    def push(self, value):
        self.stack.append(value)
        if not self.maxima or value >= self.maxima[-1]:
            self.maxima.append(value)
        
    def printMax(self):
        print self.maxima[-1]
    
    def pop(self):
        value = self.stack.pop()
        if value == self.maxima[-1]:
            self.maxima.pop()

def main():
    cs = CustomStack()
    
    N = int(raw_input())
    
    for _ in xrange(N):
        unknown = raw_input()
        
        command = unknown[0]
        
        if command == '1':
            cmd, value = map(int, unknown.split())
            cs.push(value)
        elif command == '2':
            cs.pop()
        else:
            cs.printMax()
    


if __name__ == '__main__':
    main()
```
