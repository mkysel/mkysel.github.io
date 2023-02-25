---
title: "HackerRank 'Fraudulent Activity Notifications' Solution"
date: "2020-08-28"
categories: 
  - "bmsce-ieee-24-hour-code-a-thon"
  - "coding-challenge"
  - "hackerrank"
  - "python"
---

##### Short Problem Definition:

HackerLand National Bank has a simple policy for warning clients about possible fraudulent account activity. If the amount spent by a client on a particular day is _greater than or equal to_ 2x the client's [median](https://www.hackerrank.com/external_redirect?to=https://en.wikipedia.org/wiki/Median) spending for a trailing number of days, they send the client a notification about potential fraud. The bank doesn't send the client any notifications until they have at least that trailing number of prior days' transaction data.

##### Link

[Fraudulent Activity Notification](https://www.hackerrank.com/contests/bmsce-ieee-codeathon/challenges/fraudulent-activity-notifications/problem)

##### Complexity:

time complexity is O(N^2)

space complexity is O(N)

##### Execution:

I am not very happy with this solution, but it passes the tests, so I am posting it. This solution is running in O(N^2) due to the element removal from the running\_median. del l\[i\] is O(N). O(NlogN) would be preferable.

The expenditures are actually not very large numbers \[0..200\], so there might be space for optimization there.

##### Solution:

```python
from bisect import bisect_left, insort_left


def activityNotifications(expenditure, d):
    warnings = 0
    
    running_median = sorted(expenditure[:d])
    for i,ele in enumerate(expenditure):
        if i < d:
            continue
                            
        if d % 2 == 1:
            median = running_median[d//2]
        else:
            median = (running_median[d//2 - 1] + running_median[d//2])/float(2)
            
        if ele >= median*2:
            warnings += 1
            
        # remove previous element
        del running_median[bisect_left(running_median, expenditure[i-d])]
        
        # add new element
        insort_left(running_median, ele)

    return warnings
```
