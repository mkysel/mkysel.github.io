---
title: "Codility 'TreeHeight' Solution"
date: "2014-08-13"
categories: 
  - "codility"
  - "coding-challenge"
  - "python"
---

##### Short Problem Definition:

Compute the height of a binary link-tree.

##### Link

[TreeHeight](https://codility.com/demo/take-sample-test/tree_height)

##### Complexity:

expected worst-case time complexity is `O(N)`

expected worst-case space complexity is `O(N)`

##### Execution:

The height of a tree is the maximal height +1 of its subtrees. In this specification a tree with just the root node has a height of 0.

##### Solution:

```python

'''
class Tree(object):
  x = 0
  l = None
  r = None
'''

def getHeight(sub_T):
    if sub_T == None:
        return 0
    return max(getHeight(sub_T.l), getHeight(sub_T.r))+1

def solution(T):
    return max(getHeight(T.l), getHeight(T.r))
```
