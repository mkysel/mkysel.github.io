---
title: "HackerRank 'Counting Valleys' Solution"
date: "2020-07-26"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

Gary is an avid hiker. He tracks his hikes meticulously, paying close attention to small details like topography. During his last hike he took exactly n steps. For every step he took, he noted if it was an _uphill_, U, or a _downhill_, D step. Gary's hikes start and end at sea level and each step up or down represents a  unit change in altitude. We define the following terms:

- A _mountain_ is a sequence of consecutive steps _above_ sea level, starting with a step _up_ from sea level and ending with a step _down_ to sea level.
- A _valley_ is a sequence of consecutive steps _below_ sea level, starting with a step _down_ from sea level and ending with a step _up_ to sea level.

##### Link

[Counting Valleys](https://www.hackerrank.com/challenges/counting-valleys/problem)

##### Complexity:

time complexity is O(N)

space complexity is O(1)

##### Execution:

I am a hiker myself, so this challenge is kinda funny. The description looks complex, but realistically one only needs to calculate whether the section was above or below sea level.

##### Solution:

```python
def countingValleys(n, s):
    level = 0
    valleys = 0
    for direction in s:
        if direction == "U":
            level += 1
            if level == 0:
                valleys += 1
        else:
            level -= 1
            
    return valleys
```
