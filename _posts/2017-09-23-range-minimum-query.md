---
layout: post
title: Range Minimum Query(RMQ)
comments: true
tags:
- Practice Algorithm
---


> What is a minimum value from indices 2 to 4 in the array of [-1, 2, 4, 0]?

It can be solved by using a segment tree. It is a heap-like data structure that can update or query operations in logarithmical time.

If the given array length is 4 which is a power of 2, then the size of segment tree will be `4*2 - 1 = 7`. If array's length is 5 which is not a power of 2, we should find the next power of 2 and perform, `8*2 - 1 = 15`.


element | -1  | 2   | 4   | 0
:---:   |:---:|:---:|:---:|:---:
index   | 0   | 1   | 2   | 3 

parent = floor[ (i-1) / 2 ]  
left_child = [2i + 1]  
right_child = [2i + 2]

The alternative solution is to run a loop from l to r and find the minimum but if there are millions of queries then total time would be O(m * n) where m is the number of queries and n is the size of the array.

A = [3, -1, 2]

.  | 0 | 1  | 2
:---:|:---:|:---:|:---:
 0 | 3 | -1 | -1
 1 |   | -1 | -1
 2 |   |    | 2

It answers the query in O(1) time, but it takes O(n^2) to build this matrix and it takes O(n^2) to maintain the space of this matrix. So it does not scale very well if n is a really big number. So this is where **Segment Tree** comes. It takes O(n) time to build **Segment Tree** and it takes O(n) spcae to maintain and O(log n) to answer the query.

## **References**
- [Range Minimum Query and Lowest Common Ancestor](https://www.topcoder.com/community/data-science/data-science-tutorials/range-minimum-query-and-lowest-common-ancestor/#Range_Minimum_Query_(RMQ))
- [Segment Tree Range Minimum Query](https://www.youtube.com/watch?v=ZBHKZF5w4YU)
- [Range Minimum Query](http://www.learn4master.com/algorithms/range-minimum-query-segment-tree)