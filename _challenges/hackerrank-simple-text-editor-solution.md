---
title: HackerRank 'Simple Text Editor' Solution
date: '2016-01-28'
categories:
  - coding-challenge
  - hackerrank
  - python
---

##### Short Problem Definition:

In this problem, your task is to implement a simple text editor. Initially, a file contains an empty string S. Your task is to perform Q operations consisting of the following 4 types:

1. append(W) - Appends the string W at the end of S.
1. erase(k) - Erase the last k character of S.
1. get(k) - Returns the kth character of S.
1. undo() - Undo the last not previously undone operation of type 1 or 2, so it reverts S to the state before that operation.

##### Link

[Simple Text Editor](https://www.hackerrank.com/challenges/simple-text-editor)

##### Complexity:

time complexity is

space complexity is

##### Execution:

I maintain two separate stacks. _Self.stack_ represents the current state of the text. I can read data from this stack by indexing it. _Self.cmds_ keeps the history of commands. Each command type knows how to execute itself and how to revert itself.

##### Solution:

```python

class AppendCmd():
    def execute(self, stack, value):
        self.length = len(value)
        stack.extend(value)
        
    def revert(self, stack):
        for _ in xrange(self.length):
            stack.pop()

class EraseCmd():
    def execute(self, stack, value):
        self.text = stack[-value:]
        for _ in xrange(value):
            stack.pop()
        
    def revert(self, stack):
        stack.extend(self.text)         
            
class CustomStack:
    def __init__(self):
        self.stack = []
        self.cmds = []
        
    def push(self, value):
        cmd = AppendCmd()
        cmd.execute(self.stack, value)
        self.cmds.append(cmd)
    
    def pop(self, value):
        cmd = EraseCmd()
        cmd.execute(self.stack, value)
        self.cmds.append(cmd)
        
    def get(self, value):
        print self.stack[value]
    
    def revert(self):
        cmd = self.cmds.pop()
        cmd.revert(self.stack)

def main():    
    
    cs = CustomStack()
    
    N = int(raw_input())
    
    for _ in xrange(N):
        unknown = raw_input()
        
        command = unknown[0]
        
        if command == '1':
            cmd, value = unknown.split()
            cs.push(list(value))
        elif command == '2':
            cmd, value = map(int, unknown.split())
            cs.pop(value)
        elif command == '3':
            cmd, value = map(int, unknown.split())
            cs.get(value - 1)
        else:
            cs.revert()
    

if __name__ == '__main__':
    main()
```
