---
layout: post
title:  "Baranov's ITA Walkthrough. Chapter 3"
date:   2016-12-21
excerpt: "Growth of Functions"
tag:
- Asymptotic Efficiency
---


**Growth of Functions**
{: style="text-align: center;"}

The order of growth of the running time of an algorithm gives a simple characterization of the algorithm's efficiency and also allows us to compare the relative performance of alternative algorithms. When we look at input sizes large enough to make only the order of growth of the running time relevant, we care about **asymptotic efficiency** of algorithms. That is, how it increases with the size of the input in the limit, as the size of the input increases without bound.
{: style="text-align: left;"}   

There are various kinds and flavours of asymptotic notations but these three are the ones you should definitely be familiar with:   

![image-center](/images/asymptotic.png){: .align-center}

Θ(Big-theta) notation
:	For a given function g(n), we denote by Θ(g(n)) the set of functions such that some other function f(n) belongs to if there exist positive constants c<sub>1</sub> and c<sub>2</sub> such that it can be sandwiched between c<sub>1</sub>g(n) and c<sub>2</sub>g(n), for sufficiently large n. We say that g(n) is an **asymptotically tight bound** for f(n) since it bounds it from above and below. 

O(Big-oh) notation
:	"Big-oh" provides us with only an **asymptoptic upper bound** of the function. For a given function g(n), we denote by O(g(n)) the set of functions such that some other function f(n) belongs to if there exists positive constants c and n<sub>0</sub> such that 0 ≤ f(n) ≤ cg(n) for all n ≥ n<sub>0</sub>. That is, for all values n at and to the right of n<sub>0</sub>, the value of the function f(n) is on or below cg(n).   

Using this notation, we can often describe the running time of an algorithm merely by inspecting the algorithm's overall structure. For example, the doubly nested loop structure of insertion sort yields an O(n<sup>2</sup>) upper bound on the worst-case running time. When we say "the running time is O(n<sup>2</sup>)", we mean that there is a function f(n) that is O(n<sup>2</sup>) such that for any value of n, no matter what particular input of size n is chose, the running time on that input is bounded from above by the value f(n). In other words, <span style="color:red">it won't run any slower than O(g(n))</span>.  


Ω(Big-omega) notation   
:	Just as Ω-notation provides an asymptotic upper bound on a function, "Big-omega of g of n" provdes an **asymptotic lower bounds**. For a given function g(n), we denote by O(g(n)) the set of functions such that some other function f(n) belongs to if there exists positive constants c and n<sub>0</sub> such that 0 ≤ cg(n) ≤ f(n) for all n ≥ n<sub>0</sub>;

When we say that the running time of an algorithm is Ω(g(n)), we mean that no matter what particular input of size n is chosen for each value of n, the running time on that input is at least a constant times g(n), for sufficiently large n. In other words, <span style="color:red">it won't run any faster than Ω(g(n))</span>.


It's important to note that running time of insertion sort belongs both to Ω(n) and O(n<sup>2</sup>) since it falls anywhere between a linear function of n and a quadratic function of n. Moreover, these bounds are asymptotically tight: for instance, the runnning time of insertion sort is not Ω(n^2), since there exists an input for which it runs in Θ(n) time (already sorted input). It is not contradictory, however, to say that the **worst-case** running time of insertion sort is Ω(n^2), since there exists such an input. From here it's not hard to deduce that Θ-notation implies both "Big-oh" and "Big-omega" notations.
{: style="text-align: left;"}

The rest of the chapter deals with some special cases and a bunch of math rules about logs/exponentials, things that are useful to know but can be easily looked up when needed.

That's all for Chapter 3.




















 
