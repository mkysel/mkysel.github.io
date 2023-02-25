---
title: "HackerRank 'Majority' Solution"
date: "2021-02-05"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

Neo has to save the world one last time. One of the battles has cost Neo his eyes. He has to fight the battle with the Deus ex machina.

In front of Neo is a set of **N** balls, each ball is color coded 0/1. Deus ex machina wants Neo to figure out which ball's color is the majority ( appears more than > N/2 ). If his choice of ball is correct, Deus ex machina will reach a truce and let humans live and if Neo fails to identify the ball with the majority, the entire human race would be put back into matrix.

Neo being blind won't be able to see the balls let alone figure out the ball with the majority. He seeks the Oracle's help. Neo picks any two balls from the set and asks Oracle if they are of the same color or not. If they are the same color, the Oracle answers **YES** and if they are not, Oracle answers **NO**. We will call _b_0/1 as ball b with color _0/1_ respectively. Majority exists in this set, i.e.,

##### Link

[Majority](https://www.hackerrank.com/challenges/oracle1/problem)

##### Complexity:

time complexity is O(N^2)

space complexity is O(1)

##### Requested By:

Soha

##### Execution:

I use exceptions to simplify the flow of the algorithm. The algorithm itself can be tested locally if you replace the areEqual function and create an array locally. This abstraction should make the flow of the algorithm clearer.

Disclaimer: the code gets 16/23 points. There must be more improvements possible. I am open to suggestions in the comments below.

The algorithm itself assumes that if two groups of elements of equal size are not equal they become irrelevant. Let's say that we know that elements 0 and 1 are different. Hence we never have to look at them again.

Otherwise the algorithm uses an adapted N(logN) algorithm to compare all elements.

##### Solution:

```python
#!/usr/bin/py

class MissingElementException(Exception):
    def __init__(self, a, b):
        self.a = a
        self.b = b
        
        
def areEqual(query, a ,b):
    if not a in query or not b in query[a]:
        raise MissingElementException(a, b)
    return query[a][b]

def algo(n, query):

    relevant_groups = []
    for i in xrange(0, n, 2):
        group = [i, i+1]
        relevant_groups.append(group)

    while relevant_groups:
        group_a = relevant_groups.pop(0)
        group_len = len(group_a)
        if areEqual(query, group_a[0], group_a[group_len//2]):
            while relevant_groups:
                group_b = relevant_groups.pop(0)
                group_len_b = len(group_b)
                if areEqual(query, group_b[0], group_b[group_len_b//2]):
                    if areEqual(query, group_a[0], group_b[0]):
                        group = group_a
                        group.extend(group_b)
                        relevant_groups.append(group)
                        break
                    else:
                        if group_len > group_len_b:
                            relevant_groups.append(group_a)
                            break
                        elif group_len < group_len_b:
                            relevant_groups.append(group_b)
                            break
                        else:
                            break
            if not relevant_groups:
                # this should be the answer
                return group_a[0]
    return query


def nextQuestion(n, plurality, lies, color, exact_lies, query):
    try:
        print algo(n, query)
    except MissingElementException as e:
        print "%ld % ld" % (e.a, e.b)
        
if __name__ == '__main__':
    vals = [int(i) for i in raw_input().strip().split()]
    query_size = input()
    query = {}
    for i in range(vals[0]):
        query[i] = {}

    for i in range(query_size):
        temp = [j for j in raw_input().strip().split()]
        if temp[2] == "YES":
            query[int(temp[0])][int(temp[1])] = 1
            query[int(temp[1])][int(temp[0])] = 1
        else:
            query[int(temp[0])][int(temp[1])] = 0
            query[int(temp[1])][int(temp[0])] = 0

    nextQuestion(vals[0], vals[1], vals[2], vals[3], vals[4], query)
```
