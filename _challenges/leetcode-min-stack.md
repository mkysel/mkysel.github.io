---
title: "LeetCode 'Min Stack' Solution"
date: "2024-06-03"
categories: 
  - "coding-challenge"
  - "leetcode"
  - "python"
excerpt: Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.
---

##### Short Problem Definition:

Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

Implement the MinStack class:

MinStack() initializes the stack object.

void push(int val) pushes the element val onto the stack.

void pop() removes the element on the top of the stack.

int top() gets the top element of the stack.

int getMin() retrieves the minimum element in the stack.

You must implement a solution with O(1) time complexity for each function.

#### Link

[Min Stack](https://leetcode.com/problems/min-stack/)

##### Complexity:

time complexity is `O(1)`

space complexity is `O(N)`

##### Execution:

This one is a head scratcher. It took a bit to figure it out. My brain immediately went to heaps, but they don't support a `last-inserted` operation. Plus insert is `log(N)`.

After a while I figured out that I can keep a running min and do some code gymnastics to keep it up-to-date.

I've seen solutions that have two stacks, one for the `val` and one for `min`. The code is simpler but needs twice the space.
##### Solution:

```python
class MinStack:

    def __init__(self):
        self.stack = []
        self.min_val = None

    def push(self, val: int) -> None:
        if self.min_val == None:
            self.min_val = val
        self.min_val = min(self.min_val, val)
        self.stack.append((val, self.min_val))

    def pop(self) -> None:
        ret = self.stack.pop()[0]
        if len(self.stack) == 0:
            self.min_val = None
        else:
            self.min_val = max(self.min_val, self.stack[-1][1])
        return ret

    def top(self) -> int:
        return self.stack[-1][0]

    def getMin(self) -> int:
        return self.stack[-1][1]

```

#### ChatGPT

ChatGPT can solve this problem correctly but does the inefficient two stack solution.
I suspect that is the reason why most submissions on LeetCode use two stacks...

```python
class MinStack:
    def __init__(self):
        self.stack = []
        self.min_stack = []

    def push(self, val: int) -> None:
        self.stack.append(val)
        # If min_stack is empty or the new value is less than or equal to the current minimum, push it onto min_stack
        if not self.min_stack or val <= self.min_stack[-1]:
            self.min_stack.append(val)

    def pop(self) -> None:
        if self.stack:
            val = self.stack.pop()
            # If the value being popped is the current minimum, pop it from min_stack as well
            if val == self.min_stack[-1]:
                self.min_stack.pop()

    def top(self) -> int:
        if self.stack:
            return self.stack[-1]
        return None

    def getMin(self) -> int:
        if self.min_stack:
            return self.min_stack[-1]
        return None
```