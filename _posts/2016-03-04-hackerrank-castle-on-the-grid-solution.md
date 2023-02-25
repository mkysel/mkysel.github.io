---
title: "HackerRank 'Castle on the Grid' Solution"
date: "2016-03-04"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
tags: 
  - "timed"
---

##### Short Problem Definition:

You are given a grid with both sides equal to N/N. Rows and columns are numbered from 0/0 to N−1/N−1. There is a castle on the intersection of the aath row and the bbth column.

Your task is to calculate the minimum number of steps it would take to move the castle from its initial position to the goal position (c/d).

It is guaranteed that it is possible to reach the goal position from the initial position.

##### Link

[Castle on the Grid](https://www.hackerrank.com/challenges/castle-on-the-grid)

##### Complexity:

time complexity is O(N^2) or O(N^3)

space complexity is O(N^2)

##### Execution:

This solution works with the task as a 2D array. There are options available where you treat the task as a graph problem. In both cases each node it visited exactly once using BFS. On each node, I generate all nodes that are connected to this node in a straight line that is not broken by a 'X'. I keep the distance data (integer) in the array itself. I store the nodes that have to be visited in a FIFO queue. Once the top element in the queue is the end, I terminate the algorithm. The data stored in the array are these:

- X        - blocked
- .          - not visited yet
- (int)   - already visited. Value is the number of steps from the beginning.

##### Solution:

```python

from collections import deque

class Point:
    def __init__(self, x, y):
        self.x = x
        self.y = y
    
    def __str__(self):
        return "X=%d,Y=%d" % (self.x, self.y)

def getPointsFromPoint(N, arr, point):
    x = point.x
    y = point.y
    points = []
    
    while x > 0:
        x -= 1
        if arr[x][y] == 'X':
            break
        points.append(Point(x,y))
    
    x = point.x
    while x < N-1: 
        x += 1 
        if arr[x][y] == 'X': 
            break 
        points.append(Point(x,y)) 
    
    x = point.x 
    while y > 0:
        y -= 1
        if arr[x][y] == 'X':
            break
        points.append(Point(x,y))
    
    y = point.y
    while y < N-1:
        y += 1
        if arr[x][y] == 'X':
            break
        points.append(Point(x,y))
        
    return points
    
def solveCastleGrid(N, arr, start, end):
    q = deque([start])
    arr[start.x][start.y] = 0
    
    while q:
        current_point = q.pop()
        current_distance = arr[current_point.x][current_point.y]
        
        points = getPointsFromPoint(N, arr, current_point)
        for p in points:
            if arr[p.x][p.y] == '.':
                arr[p.x][p.y] = current_distance + 1
                q.appendleft(p)
                if p.x == end.x and p.y == end.y:
                    return current_distance + 1
    return -1
    
if __name__ == '__main__':
    N = input()
    arr = [0] * N
    
    for i in xrange(N):
        arr[i] = list(raw_input())
        
    start_x, start_y, end_x, end_y = map(int, raw_input().split())
    
    print solveCastleGrid(N, arr, Point(start_x, start_y), Point(end_x, end_y))
```
