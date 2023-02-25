---
title: "HackerRank 'Poisonous Plants' Solution"
date: "2016-02-11"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

There are NN plants in a garden. Each of these plants has been added with some amount of pesticide. After each day, if any plant has more pesticide than the plant at its left, being weaker than the left one, it dies. You are given the initial values of the pesticide in each plant. Print the number of days after which no plant dies, i.e. the time after which there are no plants with more pesticide content than the plant to their left.

##### Link

[Poisonous Plants](https://www.hackerrank.com/challenges/poisonous-plants)

##### Complexity:

time complexity is O(N)

space complexity is O(N)

##### Execution:

 

This challenge was really hard. I could not figure it out for a long time. The part with the stack is pretty obvious, but I missed the fact that older plants can have higher daysAlive values.

##### Solution:

```python

class Plant:
    def __init__(self, pesticide, days):
        self.pesticide = pesticide
        self.days = days

def solvePlants(a):
    stack = []
    maxDaysAlive = 0
    
    for pesticide in a:
        daysAlive = 0
        while stack and pesticide <= stack[-1].pesticide:
            daysAlive = max(daysAlive, stack.pop().days)
            
        if not stack:
            daysAlive = 0
        else:
            daysAlive += 1
            
        maxDaysAlive = max(maxDaysAlive, daysAlive)
        
        stack.append(Plant(pesticide, daysAlive))
    
    print maxDaysAlive

def main():
    N = input()
     
    numbers = map(int, raw_input().split())
     
    solvePlants(numbers)
     
 
if __name__ == '__main__':
    main()
```
