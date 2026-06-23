---
title: "LeetCode 'Merge Sorted Array' Solution"
date: "2024-05-30"
categories: 
  - "coding-challenge"
  - "leetcode"
  - "python"
excerpt: Merge nums1 and nums2 into a single array sorted in non-decreasing order.
---

##### Short Problem Definition:

ou are given two integer arrays nums1 and nums2, sorted in non-decreasing order, and two integers m and n, representing the number of elements in nums1 and nums2 respectively.

Merge nums1 and nums2 into a single array sorted in non-decreasing order.

The final sorted array should not be returned by the function, but instead be stored inside the array nums1. To accommodate this, nums1 has a length of m + n, where the first m elements denote the elements that should be merged, and the last n elements are set to 0 and should be ignored. nums2 has a length of n. Write a function called merge in python.

##### Link

[Merge Sorted Array](https://leetcode.com/problems/merge-sorted-array)

##### Complexity:

time complexity is `O(N+M)`

space complexity is `O(1)`

##### Execution:

I've seen the use of `sort` in a lot of the solutions but that puts us at `O(nlogn)` which we want to avoid.
We can't start from the beginning because that would require moving things around in nums1.

This submission used less memory than most others.

##### Solution:

```python
from typing import List

def merge(nums1: List[int], m: int, nums2: List[int], n: int):
    idx1 = m-1
    idx2 = n-1

    for tail in range(m+n-1, -1, -1):
        if idx1 < 0:
            nums1[tail] = nums2[idx2]
            idx2 -= 1
        elif idx2 < 0:
            nums1[tail] = nums1[idx1]
            idx1 -= 1
        elif nums1[idx1] >= nums2[idx2]:
            nums1[tail] = nums1[idx1]
            idx1 -= 1
        else:
            nums1[tail] = nums2[idx2]
            idx2 -= 1
```

#### ChatGPT

ChatGPT can solve this problem correctly and efficiently:
```python
def merge(nums1, m, nums2, n):
    """
    Merges two sorted arrays nums1 and nums2 into one sorted array within nums1.
    
    Parameters:
    nums1 (List[int]): The first sorted array with a length of m + n.
    m (int): The number of elements in nums1 that should be merged.
    nums2 (List[int]): The second sorted array.
    n (int): The number of elements in nums2.
    """
    # Start from the end of nums1 and nums2
    i = m - 1
    j = n - 1
    k = m + n - 1
    
    # While there are still elements in both nums1 and nums2
    while i >= 0 and j >= 0:
        if nums1[i] > nums2[j]:
            nums1[k] = nums1[i]
            i -= 1
        else:
            nums1[k] = nums2[j]
            j -= 1
        k -= 1
    
    # If there are still elements in nums2, copy them
    while j >= 0:
        nums1[k] = nums2[j]
        j -= 1
        k -= 1

# Example usage:
nums1 = [1, 2, 3, 0, 0, 0]
m = 3
nums2 = [2, 5, 6]
n = 3
merge(nums1, m, nums2, n)
print(nums1)  # Output should be [1, 2, 2, 3, 5, 6]
```