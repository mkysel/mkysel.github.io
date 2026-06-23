---
title: "Codility 'SqlSegmentsSum' Kalium 2015 Solution"
date: "2015-05-07"
categories: 
  - "codility"
  - "coding-challenge"
  - "python"
---

##### Short Problem Definition:

Compute the total length covered by 1-dimensional segments.

##### Link

[SqlSegmentSum](https://codility.com/demo/take-sample-test/kalium2015/)

##### Complexity:

time complexity

space complexity

##### Execution:

I first create a numbers table from 1 to 1.000.000, this could be done just once when creating the DB. In this challenge it adds a flat 0.3sec runtime to all testcases.

In the second step I simply unique join the segments and number tables.

##### Solution:
```sql
-- create numbers utility structure, very basic integer primary key column n
create table numbers(n integer not null, primary key(n asc));
-- insert generated sequential integers
insert into numbers(n)
select rowid
from (

-- build 1,000,000 rows using Cartesian product
         select 1
         from (
                  select 0 union select 1 union select 2 union select 3
                  union select 4 union select 5 union select 6
                  union select 7 union select 8 union select 9
              ) a, (
                  select 0 union select 1 union select 2 union select 3
                  union select 4 union select 5 union select 6
                  union select 7 union select 8 union select 9
              ) b, (
                  select 0 union select 1 union select 2 union select 3
                  union select 4 union select 5 union select 6
                  union select 7 union select 8 union select 9
              ) c, (
                  select 0 union select 1 union select 2 union select 3
                  union select 4 union select 5 union select 6
                  union select 7 union select 8 union select 9
              ) d, (
                  select 0 union select 1 union select 2 union select 3
                  union select 4 union select 5 union select 6
                  union select 7 union select 8 union select 9
              ) e, (
                  select 0 union select 1 union select 2 union select 3
                  union select 4 union select 5 union select 6
                  union select 7 union select 8 union select 9
              ) f

     ) derived;

select count(distinct n) from numbers inner join segments
                                                 on numbers.n between segments.l+1 and segments.r
```