---
title: "HackerRank 'The Love-Letter Mystery' Solution"
date: "2015-02-28"
categories: 
  - "c"
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

James found a love letter his friend Harry has written for his girlfriend. James is a prankster, so he decides to meddle with the letter. He changes all the words in the letter into palindromes.

##### Link

[The Love-Letter Mystery](https://www.hackerrank.com/challenges/the-love-letter-mystery)

##### Complexity:

time complexity is O(N\*T);

space complexity is O(1)

##### Execution:

You process the string from the beginning towards mid and always decrement the higher side.

##### Solution:

```python

#!/usr/bin/py
if __name__ == '__main__':
    t = input()
    for _ in range(t):
        s = map(int, raw_input().split())
        reductions = 0
    	for i in range(0,len(s)//2):
            reductions += abs(ord(s[i]) - ord(s[-1-i]))
    	print reductions 
```

* * *

```cpp

#include <cmath>
#include <cstdio>
#include <vector>
#include <iostream>
#include <algorithm>
using namespace std;


int letterMystery(string &str){
    int changes = 0;
    
    int mid = str.length()/2;
    int len = str.length();
    
    for (int i=0; i < mid; i++){
        int diff = abs(int(str.at(i)) - int(str.at(len-i-1)));
        changes += diff;
    }
    
    return changes;
}
int main() {
    string str;
    int t;
    cin>>t;
    for (int i=0;i<t;i++) {
        cin>>str;
        cout << letterMystery(str) << endl;
  }
  return 0;
}
```
