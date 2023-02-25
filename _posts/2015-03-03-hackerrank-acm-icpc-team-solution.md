---
title: "HackerRank 'ACM ICPC Team' Solution"
date: "2015-03-03"
categories: 
  - "c"
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

You are given a list of N people who are attending ACM-ICPC World Finals. Each of them are either well versed in a topic or they are not. Find out the maximum number of topics a 2-person team can know. And also find out how many teams can know that maximum number of topics.

##### Link

[ACM ICPC Team](https://www.hackerrank.com/challenges/acm-icpc-team)

##### Complexity:

time complexity is O(N^3);

space complexity is O(N)

##### Execution:

I generate all teams by brute force.

##### Solution:

```python
def acmTeam(n, m, topic):
    maxKnown = 0
    teamsCnt = 0
    for i in xrange(n-1): 
        for j in xrange(i+1, n):
            knownForThisCombo = 0
            for t in xrange(m):
                if topic[i][t] == '1' or topic[j][t] == '1':
                    knownForThisCombo += 1
            if knownForThisCombo > maxKnown:
                maxKnown = knownForThisCombo;
                teamsCnt = 1;
            elif knownForThisCombo == maxKnown:
                teamsCnt += 1;
    
    return maxKnown, teamsCnt
```

```cpp
#include <cmath>
#include <cstdio>
#include <vector>
#include <iostream>
#include <algorithm>
using namespace std;



int acmIcpc(string &str1, string &str2, int length){
    int topicsKnown = 0;
    for (int i=0; i<length; i++){
        if (str1.at(i) == '1' or str2.at(i) == '1'){
            topicsKnown+= 1;
        }
    }
    return topicsKnown;
}
int main() {
    string str[500] = {};
    int N, M;
    cin>>N>>M;
    for (int i=0;i<N;i++) {
        cin>>str[i];
    }
    
    int maxKnown = 0, teamsCnt = 0;
    
    for (int i=0;i<N-1;i++) {
        for (int j=i+1;j<N;j++) {
            int knownForThisCombo = acmIcpc(str[i], str[j], M);
            if (knownForThisCombo > maxKnown){
                maxKnown = knownForThisCombo;
                teamsCnt = 1;
            } else if (knownForThisCombo == maxKnown){
                teamsCnt += 1;
            }
        }
    }
    
    cout << maxKnown << endl;
    cout << teamsCnt << endl;
    return 0;
}
```
