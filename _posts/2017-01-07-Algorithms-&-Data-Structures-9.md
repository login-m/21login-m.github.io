---
layout: post
title:  "ITA Baranov's Walkthrough. Chapter 9"
date:   2017-01-07
excerpt: "Medians and Order Statistics"
tag:
- Selection Problem
---

**Medians and Order Statistics**
{: style="text-align: center;"}

![image-center](/images/ostat.png){: .align-center}

The ith **order statistic** of a set of n elements is the ith smallest element. For example, the minimum of a set of elements is the first order statistic (i = 1), and the maximum is the nth order statistic (i = n). A median, informally, is the "halfway point" of the set. When n is odd, the median is unique, occurring at  i = (n+1)/2. When n is even, there are two medians, occurring at i = n/2 and i = n/2+1. Next, the chapter addresses the problem of selecting the ith order statistic from a set of n distinct numbers. We formally specify the **selection problem** as follows:


**Input**: A set A of n (distinct) numbers and an integer i, with 1 ≤ i ≤ n  

**Output**: The element x is in set A that is larger than exactly i - 1 other elements of A.

We can no doubt solve the problem using any of comparison sorts presented earlier in O(nlgn) time. However, there is a much faster solution.   

**Selection in linear time**
{: style="text-align: center;"}


To solve this problem, the book presents a divide-and-conquer algorithm. The algorithm <code>RANDOMIZED-SELECT</code> is modeled after the quicksort algorithm of Chapter 7. As in quicksort, we partition the input array recursively. But unlike quicksort, which recursively processes both sides of the partition, <code>RANDOMIZED-SELECT</code> works on only one side of the partition. This difference makes the latter to have the expected running time of Θ(n), assuming that the elements are distinct. 

<pre>
RANDOMIZED-SELECT(A,p,r,i)
if p == r     // base case
	return A[p]
q = RANDOMIZED-PARTITION(A,p,r) // partition
k = q - p + 1 // left side + 1 (pivot)
if i == k  //the pivot value is the answer
	return A[q] 
elseif i < k  // the answer is in the front 
	return RANDOMIZED-SELECT(A,p,q-1,i)
else          // the answer is in the back half 
	return RANDOMIZED-SELECT(A,q+1,r,i-k)
</pre>

By doing some math, the book concludes that we can find any order statistic, and in particular the median, in **expected linear time**, assuming that the elements are distinct. There is also an algorithm for selection problem that has a **worst-case linear time** but I won't cover it here since it's not as practical as this one.