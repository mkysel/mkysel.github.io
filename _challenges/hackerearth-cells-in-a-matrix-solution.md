---
title: "HackerEarth 'Cells in a matrix' Solution"
date: "2020-10-25"
categories: 
  - "coding-challenge"
  - "hackerearth"
  - "python"
---

##### Short Problem Definition:

You are given an integer N denoting an N×N matrix. Initially, each cell of the matrix is empty. You are given K tasks. In each task, you are given a cell (i,j) where cell (i,j) represents the ith row and jth column of the given matrix.

You have to perform each task sequentially in the given order. Each task is described in cell (i,j). For each task, you have to place X in each cell of row i and each cell column j. After you complete each task, you are required to print the number of empty cells in the matrix. 

##### Link

[Cells In A Matrix](https://www.hackerearth.com/practice/data-structures/hash-tables/basics-of-hash-tables/practice-problems/algorithm/easy-23-6031def9/description/)

##### Complexity:

time complexity is `O(K\*log(N))`

space complexity is `O(N)`

##### Execution:

There are many solutions to this problem statement. I picked this one, since its easy to understand and is correct.

##### Solution:

```python
def cells_sol (N, K, tasks):
    rows = set([i+1 for i in xrange(N)])
    columns = set([i+1 for i in xrange(N)])
    result = []

    for task in tasks:
        rows.discard(task[0])
        columns.discard(task[1])
        result.append(len(rows)*len(columns))

    return result
    

N, K = map(int, raw_input().split())
task = []
for _ in range(K):
    i, j = map(int, raw_input().split())
    X = i, j
    task.append(X)


out_ = cells_sol(N, K, task)
print (' '.join(map(str, out_)))
```
