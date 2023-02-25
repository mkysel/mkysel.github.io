---
title: "HackerRank 'Service Lane' Solution"
date: "2015-03-18"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

Calvin is driving his favorite vehicle on the 101 freeway. He notices that the check engine light of his vehicle is on, and he wants to service it immediately to avoid any risks. Luckily, a service lane runs parallel to the highway. The length of the highway and the service lane is N units. The service lane consists of N segments of equal length and different width.

##### Link

[Service Lane](https://www.hackerrank.com/challenges/service-lane)

##### Complexity:

time complexity is `O(n)`

space complexity is `O(n)`

##### Execution:

This challenge can be solved in a brute force manner. You just check for every entry/exit combination the biggest possible vehicle by min(arr\[entry:exit+1\]). Here, I will present a prefix sum solution with better runtime.

I create a prefix array, that saves the last entry point (or -1 if not possible) for this type of vehicle that comes before the current index (the exit point). Therefore, I can check for every entry/exit point the biggest possible type in constant time.

##### Solution:

```python

#read
n,t = raw_input().split()
n = int(n)
t = int(t)
arr = map(int, raw_input().split())

#preprocess
last_possible_entry = []
for i, lane_width in enumerate(arr):
    last_car_entry = -1
    last_truck_entry = -1
    if lane_width > 1:
        last_car_entry = i
        if len(last_possible_entry) > 0 and last_possible_entry[-1][0] >= 0:
            last_car_entry = last_possible_entry[-1][0]
    
    if lane_width > 2:
        last_truck_entry = i
        if len(last_possible_entry) > 0 and last_possible_entry[-1][1] >= 0:
            last_truck_entry = last_possible_entry[-1][1]
       
    last_possible_entry.append([last_car_entry, last_truck_entry])

#get test cases    
for _ in xrange(t):
    i, j = map(int, raw_input().split())
    
    access_pattern = last_possible_entry[j]
    if access_pattern[1] != -1 and access_pattern[1] <= i:
        print "3"
    elif access_pattern[0] != -1 and access_pattern[0] <= i:
        print "2"
    else:
        print "1"
    
    
```
