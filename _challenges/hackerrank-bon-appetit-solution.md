---
title: "HackerRank 'Bon Appétit' Solution"
date: "2020-07-26"
categories: 
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

Anna and Brian are sharing a meal at a restaurant and they agree to split the bill equally. Brian wants to order something that Anna is allergic to though, and they agree that Anna won't pay for that item. Brian gets the check and calculates Anna's portion. You must determine if his calculation is correct.

##### Link

[Bon Appétit](https://www.hackerrank.com/challenges/bon-appetit/problem)

##### Complexity:

time complexity is `O(N)`

space complexity is `O(N)`

##### Execution:

Anna is being fairly difficult. I hope that their relationship is otherwise healthy. As for the code: just follow the description.

##### Solution:

```python
def bonAppetit(bill, k, b):
    should_have = (sum(bill) - bill[k])/2
    diff = b - should_have
    if (not diff):
        return "Bon Appetit"
    else:
        return diff
```
