---
title: "HackerRank 'Game of Thrones - I' Solution"
date: "2015-03-02"
categories: 
  - "c"
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

Dothraki are planning an attack to usurp King Robert from his kingdom. King Robert learns of this conspiracy from Raven and plans to lock the single door through which an enemy can enter his kingdom. But, to lock the door he needs a key that is an anagram of a certain palindrome string.

##### Link

[Game of Thrones - I](https://www.hackerrank.com/challenges/game-of-thrones)

##### Complexity:

time complexity is `O(N)`

space complexity is `O(1)`

##### Execution:

A palindrome must by definition have an even number of letters. The only exception is a string of an odd length ('aba') that has exactly one odd letter count.

##### Solution:

```python

#!/usr/bin/py
if __name__ == '__main__':
    string = raw_input()
    found = True
    # Write the code to find the required palindrome and then assign the variable 'found' a value of True or False
    oddCnt = 0
    letterCnt = [0] * 26
    
    for letter in string:
        letterCnt[ord(letter)-ord('a')] += 1
    
    for cnt in letterCnt:
        oddCnt += cnt % 2
        
    if oddCnt > 1:
        found = False

    if not found:
        print("NO")
    else:
        print("YES")
```

* * *

```cpp

#include <cmath>
#include <cstdio>
#include <vector>
#include <iostream>
#include <algorithm>
#include <string>
using namespace std;


int main() {
   
    string s;
    cin>>s;
     
    int flag = 1;
    
    int oddCnt = 0;
    int letterCnt[26] = {};
        
    for (int i=0; i<s.length(); i++){
        letterCnt[int(s[i]) - int('a')]++;
    }
    
    for (int i=0; i<26; i++){
        oddCnt += letterCnt[i] % 2;
    }
    if (oddCnt > 1){
        flag = 0;
    }
    
    // Assign Flag a value of 0 or 1 depending on whether or not you find what you are looking for, in the given string 
    if(flag==0)
        cout<<"NO";
    else
        cout<<"YES";
    return 0;
}
```
