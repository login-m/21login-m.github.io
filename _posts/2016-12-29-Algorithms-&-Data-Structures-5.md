---
layout: post
title:  "ITA Baranov's Walkthrough. Chapter 5"
date:   2016-12-28
excerpt: "Probabilistic Analysis & Randomized Algorithms"
tag:
- Probabilistic Analysis
- Randomized Algorithms
---

**The hiring problem**
{: style="text-align: center;"}

Suppose that you need to hire a new office assistant and you decide to use an employment agency. It sends you one candidate each day. The cost of interview is small compared to the actual fees you would have to pay if you decide on a person. You are committed to having, at all times, the best possible person for this job. Therefore, if you decide that the applicant is better qualified than the current office assistant, you will fire the current office assistant and hire the new applicant. You wish to estimate the cost of this strategy.

<pre>
HIRE-ASSISTANT(n)
best = 0  //candidate 0 is a least-qualified dummy candidate
for i = 1 to n
	interview candidate i
	if candidate i is better than candidate best
		best = i
		hire candidate i
</pre>

Analyzing the cost of algorithm may seem very different from analyzing the running time. However, the analytical techniques used are identical whether we are analyzing cost or running time. We need to count the number of times certain basic operations are executed.
Interviewing has a low cost **c<sub>i</sub>**, whereas hiring is expensive, costing **c<sub>h</sub>**. If m is the number of people hired, the total cost associated with this algoritm is O(c<sub>i</sub>n + c<sub>h</sub>m).   

**Worst-case analysis**: we would actually need to hire every candidate that we interview. This situation occurs if the candidates come in strictly increasing order of quality, in which case we have n times, for a total hiring cost of O(c<sub>h</sub>n).    

**Probabilistic analysis**: we must use knowledge of, or make assumptions about, the distribution of the inputs. Then we analyze our algorithm computing an **average-case running time**, where we take the average over the distribution of the possible inputs. In this problem, we will assume that the applicants come in a random order.   

We wish to compute the expected number of times that we hire a new office assistant. Candidate **i** is hired, exactly when candidate i is better than each of candidates 1 through i - 1. Because we have assumed that the candidates arrive in a random order, the first i candidates have appeared in a random order. Any one of these first i candidates is equally likely to be the best-qualified so far. Candidate i has a probability of **1/i** of being better qualified than candidates  1 through i - 1 and thus a probability of 1/i of being hired. Thus,  
Σ(i = 1 to n)[1/i] = **lnn + O(1)** (Harmonic Series)   

Even though we interview n people, we actually hire only approximately lnn of them, on average. This means that the average-case total hiring cost is **O(c<sub>h</sub>ln*n)** which is a significant improvement over the worst-case hiring cost of **O(c<sub>h</sub>n)**.   


In many cases, we know very little about the input distribution. Yet we often can use probablity and randomness as a tool for algorithm design and analysis, by making the behaviour of part of the algorithm random. In the hiring problem, it may seem as if the candidates are being presented to us in a random order, but we have no way of knowing whether or not they really are. What we need to do is to change the model slightly. We say that the employment agency has n candidates, and they send us a list of the candidates in advance. On each day, we chose, randomly, which candidate to interview. Although we still know nothing about how well they suit our job, we have made a significant change. Instead of relying on a guess that the candidates come to us in a random order, we have instead gained control of the process and enforced a random order. In other words, we modified the algorithm and we still expect to hire a new office assistant approximately ln*n times. But now we expect this to be the case for **any** input, rather than for inputs drawn from a particular distribution.

<pre>
RANDOMIZED-HIRE-ASSISTANT(n)
**randomly permute the list of candidates**
best = 0  //candidate 0 is a least-qualified dummy candidate
for i = 1 to n
	interview candidate i
	if candidate i is better than candidate best
		best = i
		hire candidate i
</pre>

<pre>
PERMUTE-BY-SORTING(A)
n = A.length
let P[1..n] be a new array
for i = 1 to n
	P[i] = RANDOM(1,n<sup>3</sup>)
sort A, using P as sort keys [Usually comparison sort theta(nlgn)]
</pre>

	Example: 
A = <1,2,3,4>
P = <36,3,62,19>
B = <2,4,1,3>

<pre>
RANDOMIZE-IN-PLACE(A)
n = A.length
for i = 1 to n
	swap A[i] with A[RANDOM(i,n)] (Better, O(n) time)     
</pre>  

Again, algorithm is randomized if its behaviour is determined not only by its input but also by values produced by a random-number generator. In general, we discuss the **average-case running time** when the probability distribution is over the inputs to the algorithm, and we discuss the **expected running time** when the algorithm itself makes random choices.     

