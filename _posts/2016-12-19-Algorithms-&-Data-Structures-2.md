---
layout: post
title:  "Baranov's ITA Walkthrough. Chapter 2"
date:   2016-12-19
excerpt: "Insertion & Merge sorts. Analyzing the Algorithm"
tag:
- Insertion Sort
- Runtime Complexity
- Merge Sort
---


**Insertion Sort**
{: style="text-align: center;"}

Chapter 2 introduces **insertion sort**, an algorithm that solves the sorting problem mentioned in a previous chapter.


Input
:	A sequence of **n** numbers **<a<sub>1</sub>, a<sub>2</sub>, ..., a<sub>n</sub>>**  

Output
:	 A permutation **<b<sub>1</sub>, b<sub>2</sub>, ..., b<sub>n</sub>>** of the input sequence such that **b<sub>1</sub> ≤ b<sub>2</sub> ≤ ... ≤ b<sub>n</sub>**
{: style="text-align: left;"}

<pre>
INSERTION-SORT(A)						cost 	times
for j = 2 to A.length						c<sub>1</sub>	n
	key = A[j]						c<sub>2</sub>	n-1
	// Insert A[j] into the sorted sequence A[1..j-1].	0	n-1
	i = j - 1						c<sub>4</sub>	n-1	
	while i > 0 and A[i] > key				c<sub>5</sub>	Σ(j=2 to n) t<sub>j</sub>
	  A[i+1] = A[i]						c<sub>6</sub>	Σ(j=2 to n) t<sub>j</sub> - 1
	  i = i - 1						c<sub>7</sub>	Σ(j=2 to n) t<sub>j</sub> - 1
	A[i+1] = key						c<sub>8</sub>	n-1
</pre>

Basically, algorithm divides the sequence in two parts. The left part **A[1...j-1]** constitutes the currently sorted subarray and the right part is a subarray **A[j+1...n]** that corresponds to the numbers to be sorted. What you want to do is to pick an element (starting from 2), go down the array and try to find a good spot to fit the number in, at the same time copying the values one place over to have a free slot for insertion.
{: style="text-align: left;"}

**Analyzing algoritms**
{: style="text-align: center;"}

Running Time **T(N)**
:	The running time of the algorithm is the sum of running times for each statement executed; a statement that takes **c<sub>i</sub>** steps to execute and executes **n** times will contribute **c<sub>i</sub>n** to the total running time.  

T(N) = c<sub>1</sub>*n c<sub>2</sub>(n-1) +c<sub>4</sub>(n-1) +c<sub>5</sub>Σj +c<sub>6</sub>Σ(t<sub>j</sub>-1) +c<sub>7</sub>Σ(t<sub>j</sub>-1) +c<sub>8</sub>(n-1).
{: style="text-align: left;"}

Algorithm's running time does depend on the input. 
For example, if the input array is sorted, we would have A[i]  key when i has its initial value of j-1. Thus t<sub>j</sub> = 1 for j = 2, 3 ... n and the T(N) would be:  
T(N) = c<sub>1</sub>n +c<sub>2</sub>(n-1) +c<sub>4</sub>(n-1) +c<sub>5</sub>(n-1) +c<sub>8</sub>(n-1) = (c<sub>1</sub>+..c<sub>8</sub>)n - (c<sub>2</sub>+c<sub>4</sub>+c<sub>5</sub>+c<sub>8</sub>) = **Cn - c**  
This clearly looks like a <span style="color:red">linear</span> function of n.
{: style="text-align: left;"}

Another example, array is in reverse sorted order, then we must compare the entire left subarray which would make t<sub>j</sub> = j for j = 2, 3 ... n. Consequently, T(N) would look like:  
T(N) = c<sub>1</sub>n +c<sub>2</sub>(n-1) +c<sub>4</sub>(n-1) +c<sub>5</sub>(n(n+1)/2-1) + c<sub>6</sub>(n(n-1)/2) + c<sub>7</sub>(n(n-1)/2) +c<sub>8</sub>(n-1)   
= **Cn<sup>2</sup> + Cn - c**  
This clearly looks like a <span style="color:red">quadratic</span>  function of n.
{: style="text-align: left;"}
Notice that  Σ(j=2 to n)[j] = n(n+1)/2-1 and Σ(j=2 to n)[j-1] = n(n-1)/2
{: .notice_info}

When we try to analyze algorithms, we usually only care about finding the **worst-case** running times. Why?  
1. It gives an upper bound on the running time for any input. Knowing it provides a guarantee that the algorithm will never take any longer.  
2. In some cases, worst case occurs fairly often (e.g. searching a database for non-existent information)  
3. The "average case" is often roughly as bad as the worst case. For example, imagine that half of the elements in A[1...j-1] are less than A[j] and half are greater. It would make t<sub>j</sub> = j/2 which would be the same quadratic formula but divided by a constant (2), not a huge difference.
{: style="text-align: left;"}

To denote the worst case we write the running time of **Θ(n<sup>2</sup>)** (pronounced "theta of n-squared") which represents a rate of growth or order of growth. We can ignore the lower-order terms because they are relatively insignificant for large values of n. All we care about is the leading term of the formula (e.g., cn<sup>2</sup>).
{: style="text-align: left;"}


**Algorithm Design & Mergesort**
{: style="text-align: center;"}

Sometimes, we can come up with a different approach to tackle the same problem. So far we used an incremental approach: to sort the subarray A[1...j-1], we inserted a single element A[j] into its proper place, yielding the sorted subarray. A different way of doing things would be using **divide-and-conquer** method: break the problem into several subproblems that are similar to the original problem but smaller in size, solve the subproblems recursively, and finally combine these solutions to create a solution to the original problem. Mergesort algorithm would be a good example of this.
{: style="text-align: left;"}

**Divide**: Divide the n-element sequence to be sorted into two subsequences of n/2 elements each.  
**Conquer**: Sort the two subsequences recursively using merge sort.   
**Combine**: Merge the two sorted subsequences to produce a fully sorted sequence.   
{: style="text-align: left;"}

The following <code>MERGE</code> function merges two sorted subarrays **A[p..q]** and **A[q + 1..r]** where p ≤ q ≤ r. It merges them to form a single sorted subarray that replaces the current subarray **A[p..r]**. Since we perform at most n steps, where each step is just a comparing of two elements from left and right subarrays, merging takes Θ(N) time.
{: style="text-align: left;"}

<pre>
MERGE(A,p,q,r)
n1 = q-p+1
n2 = r-q
let L[1..n1+1] and R[1..n2 +1] be new arrays
for i = 1 to n1
	L[i] = A[p+i-1]
for j=1 to n2
	R[j] = A[q+j]
L[n1+1] = inf
L[n2+1] = inf
i = 1
j = 1
for k = p to r
	if L[i] <= R[j]
		A[k] = L[i]
		i = i + 1
	else A[k] = R[j]
		j = j + 1
</pre>

1. Compute the length of **A[p..q]** and **A[q+1..r]**.
2. Create left and right of lengths **n<sub>1</sub>+1** and **n<sub>2</sub>+1**. Extra position holds the sentinel which would simplify our code.
3. For loops copy the subarray **A[p..q]** into **L[1..n<sub>1</sub>]** and **A[q+1..r]** into **R[1..n<sub>2</sub>]**.
4. Put the sentinels.
5. Put the values in **A** in order.
{: style="text-align: left;"}

Now we can use merge procedure as a subroutine in the merge sort algorithm.
The procedure sorts the elements in the subarray **A[p..r]**. If p ≥ r the subarray has at most one element and is therefore already sorted. Otherwise, the divide step simply computes an index **q** that partitions A[p..r] into two subarrays: A[p..q], containing **ceil(n/2)** elements and A[q+1..r], containing **floor(n/2)** elements.
{: style="text-align: left;"}
<pre>
MERGE-SORT(A,p,r)
if p < r
	q = floor((p+r)/2)
	MERGE-SORT(A,p,q)
	MERGE-SORT(A,q+1,r)
	MERGE(A,p,q,r)
</pre>
To sort an entire array, we make the initial call <code>MERGE-SORT(A,1,A.length)</code>. The algorithms goes all the way to the bottom, merges pairs of 1-item seqs to form sorted seqs of length 2, merges pairs of seqs of length 2 to form sorted seqs of length 4 and so on, until two sequences of length n/2 are merged to form the final sorted sequence of length n.
{: style="text-align: left;"}

![image-center](/images/mergesort.png){: .align-center}


**Runtime Complexity**
{: style="text-align: center;"}
T(n) is either going to be Θ(1) if n ≤ c or aT(n/b) + D(n) + C(n).   
In our case:   
1) D(n) is Θ(1) operation (finding q) [Divide]   
2) We recursively solve two subproblems, each of size n/2, which contributes to 2T(n/2) to the running time [Conqueur]   
3) We have seen that MERGE takes Θ(N) time on n-element subarray so C(N) = Θ(N) [Combine]  
Thus, we get T(n) = Θ(1) [if n ≤ c] or 2T*(n/2) + Θ(n) [if n > 1]
{: style="text-align: left;"}

So what exactly is 2T*(n/2)? To find it out, let's refer to a recursion tree to better understand the first term.  

Recursion Tree
:	A recursion tree is useful for visualizing what happens when a recurrence is iterated. It diagrams the tree of recursive calls and the amount of work done at each call.  

![image-center](/images/mergesort_rt.jpg){: .align-center}

First level takes cn time.   
Second level takes 2(cn/2) = cn.      
Third level takes 4(cn/4) = cn.     
To find out the total running time we need to sum up all the costs on each level. How do we know the number of levels?

Number of levels of the recursion tree
:	The total number of levels of the recursion tree is **lgn + 1**, where n is the number of leaves, corresponding to the input size.     

Then, our recursion tree has lgn + 1 levels, each costing cn, for a total of cn(lgn + 1) = cnlgn + cn.   
Substitute this in our previous T(n) equation and we get:   
T(n) = Θ(1) [if n ≤ c] or cnlgn + cn + Θ(n) = Θ(nlgn) [if n > 1].  
This is a much better runtime complexity than the insertion sort we looked at earlier on.
{: style="text-align: left;"}

 
