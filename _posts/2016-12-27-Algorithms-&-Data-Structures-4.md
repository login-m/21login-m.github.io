---
layout: post
title:  "Baranov's ITA Walkthrough. Chapter 4"
date:   2016-12-27
excerpt: "More about Divide-and-Conquer"
tag:
- Maximum Subarray Problem
- Solving Recurrences
---

**The maximum-subarray problem**
{: style="text-align: center;"}
 
 ![image-center](/images/maxsubarray1.png){: .align-center}

The maximum subarray problem is the task of finding the contiguous subarray within a one-dimensional array of numbers which has the largest sum. The trick is that the numbers can be negative, otherwise it would be too easy. Let's think how can we approach this problem using divide-and-conquer technique. Probably, we should divide the array in two parts, left and right. Then our maximum subarray must either be:   
1) entirely in the subarray A[low..mid], so that low ≤ i ≤ j ≤ mid,   
2) entirely in the subarray A[mid+1..high], so that mid ≤ i ≤ j ≤ high, or   
3) crossing the midpoint, so that low ≤ i ≤ mid **<** j ≤ high   

 ![image-center](/images/maxsubarray2.png){: .align-center}

Since the last part would not really be a smaller instance of our original problem, because it has the added restriction that the subarray it chooses must cross the midpoint, we should consider it to be in the combine part of our divide-and-conquer technique. All we need to do is to find 4.4(b) maximum subarrays of the form A[i..mid] and A[mid+1..j] and then combine them together. 


<pre>
FIND-MAX-CROSSING-SUBARRAY(A,low,mid,high)
left-sum = -inf
sum = 0
for i = mid downto low
	sum = sum + A[i]
	if sum > left-sum
		left-sum = sum
		max-left = i
right-sum = -inf
sum = 0
for j = mid + 1 to high
	sum = sum + A[j]
	if sum > right-sum
		right-sum = sum
		max-right = j
return (max-left,max-right,left-sum + right-sum);
</pre>


If the subarray A[low..high] contains n entries (so that n = high - low + 1), the call <code>FIND-MAX-CROSSING-SUBARRAY(A,low,mid,high)</code> takes Θ(n) time. Since each iteration of each of the two for loops takes Θ(1) time, we just need to count up how many iterations there are altogether. We have:  
(left) + (right) = (mid - low + 1) + (high - mid) = high - low + 1 = n 

Now we are ready to start writing our divide-and-conquer algorithm to solve the problem:
<pre>
FIND-MAXIMUM-SUBARRAY(A,low,high)
if high==low [Base Case: only one element]
	return (low,high,A[low])
else 
	mid = floor((low+high)/2)
	(left-low,left-high,left-sum) = 
		FIND-MAXIMUM-SUBARRAY(A,low,mid)
	(right-low,right-high,right-sum) = 
		FIND-MAXIMUM-SUBARRAY(A,mid+1,high)
	(cross-low,cross-high,cross-sum) = 
		FIND-MAX-CROSSING-SUBARRAY(A,low,mid,high)
	if left-sum >= right-sum and left-sum >= cross-sum
		return (left-low,left-high,left-sum)
	elseif right-sum >= left-sum and right-sum >= cross-sum
		return (right-low, right-high, right-sum)
	else return (cross-low, cross-high,cross-sum)
</pre>


Let's setup a recurrence that describes the running time of this function:   
T(n): Θ(1) + 2T(n/2) + Θ(n) + Θ(1) = 2T(n/2) + Θ(n).  
Since it looks the same as the recurrence for the merge-sort algorithm discussed in Chapter 2, we know that the solution should be T(n) = Θ(nlgn);


**Recurrences**
{: style="text-align: center;"}

Recurrence
:	An equation of inequality that describes a function in terms of its value on smaller inputs.  

Recurrences go hand in hand with the divide-and conquer paradigm discussed in Chapter 2, because they give us a natural way to characterize the running times of such algorithms. For example, we could have a recursive algorithm that divides subproblems into unequal sizes, such as a 2/3-to-1/3 split. If the divide and combine steps take linear time, such an algorithm would give rise to the recurrence T(n) = T(2n/3) + T(n/3) + Θ(n).
{: style="text-align: left;"}   

There are three methods for solving recurrences (obtaining asymptotic Θ or O bounds on the solution): 

1. In the substitution method, we guess a bound and then use mathematical induction to prove our guess correct.

2. The recursion-tree method converts the recurrence into a tree whose nodes represenet the costs incurred at various levels of the recursion.

3. The master method provides bounds for recurrences of the form T(n) = aT(n/b) + f(n) - where algorithm creates subproblems, each of which is n/b the size of the original problem, and in which the divide and combine steps together take f(n) time.   


**The substitution method for solving recurrences**
{: style="text-align: center;"}

1. Guess the form of the solution.   
2. Use mathematical induction to find the constants and show that the solution works.  

For example, T(n) = 2T(floor(n/2)) + n. We may guess that the solution is T(n) = O(nlgn) since it reminds us about the recurrence we've seen earlier. The substitution method requires us to prove that T(n) ≤ cnlgn for an appropriate choice of the constant c > 0.We start by assuming that this bound holds for all positive number m < n, in particular for m = (floor(n/2)), yiedling  T((floor(n/2)) ≤ c(floor(n/2))lg(floor(n/2)). Substitute this into recurrence and we get:  
T(n)   
	 ≤ 2(c(floor(n/2))lg(floor(n/2))) + n   
     ≤ cnlg(n/2) + n   
     = cnlgn - cnlg2 + n   
     = cnlgn - cn + n
     ≤ cnlgn   

An easy way to avoid a second step would be to pick a **c** that makes this inequality come true for sufficiently large n. As we can see, it holds if c ≥ 1. However, a right way to do it is to use mathematical induction to prove base and inductive steps, which I am not going to talk through since we would make this chapter even broader.   

**The recursion-tree method for solving recurrences**
{: style="text-align: center;"}

![image-center](/images/mergesort_rt.jpg){: .align-center}

We have already seen this method in Chapter 2 when we tried to derive the running time for the merge-sort algorithm. There are few things to remember:   

1. The subproblem decrease by a factor of **a** each time we go down one level, we eventually must reach a boundary condition. How far from the root do we reach one? The subproblem for a node at depth i is **n/a<sup>i</sup>**. This, the subproblem size hits n = 1 when **n/a<sup>i</sup> = 1** or when **i = log<sub>a</sub>n**. Thus, the tree has **log<sub>a</sub>n + 1** levels.

2. What's the cost at each level of the tree? Each level has **b** times more nodes that the level above, and so the number of nodes at depth i is **b<sup>i</sup>**. Since subproblem sizes reduce by a factor of a for each level we go down from the root, each node at depth i, has a cost of **F(n/a<sup>i</sup>)**. Multiplying this by the number of nodes at each level we get **b<sup>i</sup>*F(n/a<sup>i</sup>)**. The bottom level, at depth log<sub>a</sub>n, has **b<sup>(log<sub>a</sub>n)</sup> = n<sup>(log<sub>a</sub>b)</sup>** each contributing cost T(1).


**The master method for solving recurrences**
{: style="text-align: center;"}

Let A ≥ 1 and b > 1 be constants, let f(n) be a function, and let T(n) be defined on the nonnegative integers by the recurrence

**T(n) = aT(n/b) + f(n)**. Then T(n) has the following asymptotic bounds:

1. If f(n) = O(n<sup>log<sub>b</sub>a-e</sup>) for some constant e > 0, then T(n) = Θ(n<sup>log<sub>b</sub>a</sup>)

2. If f(n) = Θ(n<sup>log<sub>b</sub>a</sup>), then T(n) = Θ(n<sup>log<sub>b</sub>a</sup> * lgn)

3. If f(n) = Ω(n<sup>log<sub>b</sub>a+e</sup>) for some constant e &gt 0, and if af(n/b) ≤ cf(n) for some constant c less than 1 and all sufficiently large n, then T(n) = Θ(f(n))

In each of three cases, we compare the function f(n) with the function (n<sup>log<sub>b</sub>a</sup>). The largest of the two would be the solution to the recurrence. 

Important note: in order to use rule 1 f(n) must be **polynomially smaller** than (n<sup>log<sub>b</sub>a</sup>) [**polynomially larger** for rule 3]
{: .notice_info}

Examples:

T(n) = 9T(n/3) + n    
a = 9, b = 3, f(n) = n, and thus we have (n<sup>log<sub>b</sub>a</sup>) = Θ(n<sup>2</sup>). Since f(n) = O((n<sup>log<sub>3</sub>9-e</sup>), where e = 1, we can apply case 1 of the master theorem and conclude that the solution is T(n) = Θ(n<sup>2</sup>). An easier way to remember this might be to take the power of f(n), let's say d, and check the relationship between a and b<sup>d</sup>.
[9 > 3<sup>1</sup> -> case 1]

T(n) = T(2n/3) + 1     
(n<sup>log<sub>b</sub>a</sup>) = 1. Case 2 applies, since f(n) = Θ(n<sup>log<sub>b</sub>a</sup>) = Θ(1), and thus the solution to the recurrence is T(n) = Θ(lgn)
[1 = 3/2<sup>0</sup> = 1 -> case 2]


T(n) = 3T(n/4) + nlgn   
(n<sup>log<sub>b</sub>a</sup>) = O(n<sup>0.793</sup>). Since f(n) = Ω(n<sup>log<sub>4</sub>3+e</sup>), where e ~ 0.2, case 3 applies if we can show that the regularity condition holds for f(n). For sufficiently large n, we have that af(n/b) = 3(n/4)lg(n/4) ≤ (3/4) nlgn = cf(n) for c = 3/4. Thus, T(n) = Θ(nlgn).
[3 < 4<sup>log<sub>4</sub>3</sup> = 4<sup>0.793</sup> = 0.30021 -> case 3]















 
