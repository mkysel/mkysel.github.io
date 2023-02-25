---
title: "What I discovered using a simple challenge during interviews"
date: "2018-09-24"
categories: 
  - "programming"
tags: 
  - "featured"
---

## Can coders code?

When I was looking for a simple programming task that I can use to evaluate candidates, I had the following criteria in mind:

- solvable within 10 min
- does not require obscure algorithmic knowledge
- has potential for further discussion
- not language specific

I went through my list of simple problems (I only considered the easy ones) and I stumbled across [Pairs](http://www.martinkysel.com/hackerrank-pairs-solution/). I instantly realized, that it is the perfect question. There is a brute force solution, the specification can be expanded, we can talk about sorting, hashing, hash collisions, Big-O notation and cases when the input does not fit in memory. Even [Bloom Filters](https://en.wikipedia.org/wiki/Bloom_filter) are a viable topic of discussion.

My first interview was planned and I was thrilled. I would use the challenge and it would be great! The first three candidates I exposed to the task failed horribly. They could not implement the brute force solution O(n^2) without bugs in the code. I was _horrified_. Is the industry really this bad? Can [programmers really not program](https://blog.codinghorror.com/why-cant-programmers-program/)?

In the last 3 years I used this little coding task in almost 50 interviews and the failure rate is alarming. Only _two_ people managed to solve it flawlessly. They both accepted the offer and I have been thrilled to work with them ever since. There have been others that got stuck in the weeds but nevertheless showed an ability to think. Kudos to those! I will expand on the definition of flawlessly below.

I adjust the -pass- criteria accordingly to the person's experience (people who graduated a long time ago might not remember what Big-O is), title (did they code in the last job?), future job duties (are they supposed to code?) and the prowess with which they claim that they know how to program. Support engineers that have had limited exposure to python and shell are fine with something that resembles code. People that have been in the industry for a decade better be ready to go through at least 3 iterations of increasing complexity.

Before reading on, I suggest that you go and solve that [challenge](http://www.martinkysel.com/hackerrank-pairs-solution/). I will wait....

Solved? Good! Let's look at my variation with a deliberately vague specification.

## Problem Statement

> Given an array N of integers such as \[1,3,5\] and an integer K=2, find all pairs in N whose difference is K. You can use any language you want, as long as I can understand it.

## Resolving Ambiguity

A good candidate will instantly ask a subset of the following questions:

- is the array sorted?
- how big is the array?
- are there duplicates?
- are all numbers positive?
- what is the range of values in N?
- how much memory can I use?
- is K positive?

Less than 5 people ever asked me additional questions. Most rush to the brute force implementation. Finding pairs in a sorted array can be done in O(N) time. If the range of values is only 0-127, counting sort could be the optimal way forward.

Every software engineer should understand the problem that she is solving. Very often the problem is much easier than it seems at first. Sometimes it is the other way round and something that seems trivial blows up into a monstrosity. Figuring out the -what- should always come before writing the first line of code.

## Thinking in Abstract Algorithms

Languages such as C++ have a lot of accidental complexity. Rather than trying to solve the problem at hand, you will have to deal with nulls, end iterators and the general lack of syntactic sugar. This is the main reason why this blog is using Python even though I am a full time C++ dev. Don't even get me started on Java...

When you jump to the implementation, without having a clear plan in mind, it is very easy to get lost in the complexity of the language. You can loose sight of the forest due to all the trees.

Before you start writing any code, you should have a clear vision of the algorithm. Knowing the limitations of the language (C++ set is actually a tree rather than a hash set) is important for identifying bottle necks.

Let's look at a possible verbal explanation of the O(NlogN) solution for no duplicates; unsorted array; fits in memory; random 64 bit integers; would look like:

> As we iterate over the array, we are looking for possible pair candidates. We know that if we encounter a value V, a pair has to be either V+K or V-K. Ideally the lookup has to be as fast as possible. We also know that pairs are symmetric, 3/5 and 5/3 are both valid pairs. We should only count them once, but it does not matter which value we find first. This brings us to a conclusion, that we can iterate over the array and check whether a possible pair candidate has already been encountered. We store the values in a tree or a hashset, depending on the risk of collision.

Once you know what you are solving and how you are solving it, you can implement it. This process might result in a code like this:

```python

def pairs(k, arr):
    cnt = 0
    encountered = set()
    for v in arr:
        if v+k in encountered:
            cnt += 1
        if v-k in encountered:
            cnt += 1
        encountered.add(v)

    return cnt
```

When the algorithm comes first, the implementation tends to communicate the intention of the code much clearer.

## Engineers barely ever write code

If you have been in software engineering, you might have noticed that we barely ever write any code. We talk about code, we fight over code, we review code and most of the time, we _read_ code. Writing code is so rare, that some people might not have done it in months.

> “Indeed, the ratio of time spent reading versus writing is well over 10 to 1. We are constantly reading old code as part of the effort to write new code. ...\[Therefore,\] making it easy to read makes it easier to write.” ― Robert C. Martin, Clean Code: A Handbook of Agile Software Craftsmanship

It is therefore natural, that writing code on a whiteboard is an exceptionally stressful situation. We actually have very little exposure to writing code in the first place.

Since 2016 I have been recommending all engineers to write code in their spare time. Either find an open source project and contribute to it, start a little project of your own or work on programming challenges. I prefer the last option, since it actually exposes the person to the complexity of the job.

If you play music, you might know how important it is to know your notes, scales and arpeggios. Being consistent with the basics allows us to build on top and learn much more complex pieces. Similarly, engineers should be fluent with their basic tool: algorithms and data structures.

As with anything else, mastery comes with practice. **Practice writing code!**
