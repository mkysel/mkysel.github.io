---
title: "LeetCode 'Copy List with Random Pointer' Solution"
date: "2024-05-31"
categories: 
  - "coding-challenge"
  - "leetcode"
  - "python"
excerpt: Construct a deep copy of the list with random pointers
---

##### Short Problem Definition:

A linked list of length n is given such that each node contains an additional random pointer, which could point to any node in the list, or null.

Construct a deep copy of the list. The deep copy should consist of exactly n brand new nodes, where each new node has its value set to the value of its corresponding original node. Both the next and random pointer of the new nodes should point to new nodes in the copied list such that the pointers in the original list and copied list represent the same list state. None of the pointers in the new list should point to nodes in the original list.

#### Link

[Copy List with Random Pointer](https://leetcode.com/problems/copy-list-with-random-pointer)

##### Complexity:

time complexity is `O(N)`

space complexity is `O(N)`

##### Execution:

You could do this in three passes, but this solution compresses pass #1 and pass #2 into one.

Each object in python has a unique `id`. In other languages we could use the address in memory or the object directly.
I don't like using objects as keys in maps, so I opted for a simple unique key derived from the id.

The logic is as follow:
1. copy objects without the random pointer. Save both mappings from new to old and vice versa.
2. save the random pointers from old to random old
3. In a second pass fill in the random pointers. Use the maps to determine the links

The solution beats 95% of submissions.

##### Solution:

```python
class Solution:
    def copyRandomList(self, head: 'Optional[Node]') -> 'Optional[Node]':

        random_mapping = {}
        id_mapping = {} # new to old
        object_mapping = {} # old to new object

        copy_root = None

        ptr = head
        previous_copy_ptr = None

        while ptr != None:
            copy_object = Node(ptr.val)
            id_mapping[id(copy_object)] = id(ptr)
            object_mapping[id(ptr)] = copy_object
            if copy_root == None:
                copy_root = copy_object

            if ptr.random != None:
                random_mapping[id(ptr)] = id(ptr.random)

            ptr = ptr.next

            if previous_copy_ptr != None:
                previous_copy_ptr.next = copy_object

            previous_copy_ptr = copy_object

        ptr = copy_root

        while ptr != None:
            # find new to old
            old_id = id_mapping[id(ptr)]

            old_random_id = random_mapping.get(old_id)
            if old_random_id != None:
                new_object = object_mapping[old_random_id]
                ptr.random = new_object
            
            ptr = ptr.next

        return copy_root
```

#### ChatGPT

ChatGPT can solve this problem correctly but slowly. I suspect that the map operations are slower.
I do like the logic of setting all pointers in the second pass. It is conceptually easier to grasp.

```python
class Solution:
    def copyRandomList(self, head: 'Optional[Node]') -> 'Optional[Node]':
        if not head:
            return None

        # Dictionary to hold old nodes as keys and new nodes as values
        old_to_new = {}

        # First pass: create all the new nodes and store them in the dictionary
        current = head
        while current:
            old_to_new[current] = Node(current.val)
            current = current.next

        # Second pass: assign next and random pointers
        current = head
        while current:
            if current.next:
                old_to_new[current].next = old_to_new[current.next]
            if current.random:
                old_to_new[current].random = old_to_new[current.random]
            current = current.next

        # Return the head of the new list
        return old_to_new[head]
```