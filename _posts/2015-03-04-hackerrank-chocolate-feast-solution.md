---
title: "HackerRank 'Chocolate Feast' Solution"
date: "2015-03-04"
categories: 
  - "c"
  - "coding-challenge"
  - "hackerrank"
---

##### Short Problem Definition:

Little Bob loves chocolates, and goes to a store with $N in his pocket. The price of each chocolate is $C. The store offers a discount: for every M wrappers he gives to the store, he gets one chocolate for free. How many chocolates does Bob get to eat?

##### Link

[Chocolate Feast](https://www.hackerrank.com/challenges/chocolate-feast)

##### Complexity:

time complexity is O(N);

space complexity is O(1)

##### Execution:

Evaluate the number of wraps after each step. Do this until you have enough wraps to buy new chocolates.

If someone knows how to solve this in O(1) with a mathematical formula, let me know!

##### Solution:

```cpp
int eatenChocolades(int availableCash, int price, int wrapperDiscount){
    int eaten = 0;
    int wraps = 0;
    
    wraps = eaten = availableCash/price;
    
    while (wraps >= wrapperDiscount){
        int newlyEaten = wraps/wrapperDiscount;
        eaten += newlyEaten;
        wraps %= wrapperDiscount;
        wraps += newlyEaten;
    }
    
    
    return eaten;
}
```

```python
def chocolateFeast(availableCash, price, wrapperDiscount):
    eaten = availableCash // price
    wraps = eaten

    while wraps >= wrapperDiscount:
        newlyEaten = wraps // wrapperDiscount
        eaten += newlyEaten
        wraps = wraps % wrapperDiscount + newlyEaten

    return eaten
```
