---
layout: post
title:  "Baranov's ITA Walkthrough. Chapter 1"
date:   2016-12-17
excerpt: "Algorithms & Data Structures"
tag:
- Algorithms
- Data Structures
---


<figure class="half">
    <a href="/images/clrs.png"><img src="/images/clrs.png"></a>
</figure>


 As the semester comes to an end, we (students) start to think how to best spend a so desired winter break. Well, you really shouldn't think too hard because there is doubtly anything better than going through the [Introduction to Algorithms](https://mitpress.mit.edu/books/introduction-algorithms). It was written by those smarties at MIT so it shouldn't be a waste of time. So, my plan is to skim through the book, picking up key ideas of each chapter. My expectation is to cover half of the book's topics by the end of my holidays and continue working on it some time later. I called this "project" __Baranov's ITA Walkthrough__ because it just sounds cool.  

 Well, here we go. This is all you need to know from Chapter 1:
 {: style="text-align: left;"}

What is algorithm?
:    <span style="color:red">Algorithm</span> is any well-defined computational procedure that takes some value, or set of values, as input and produces some value, or set of values as output. In other words, a sequence of computational steps that transform the input into the output.

A different variety of problems can be solved using algorithms. This is why we (programmers) should consider them as a __technology__ alongside with computer architectures, GUIs, integrated Web technologies or object-oriented systems (actually, all these things are also built upon the algorithms, lol). 

Efficiency is a pretty big deal. Consider an insertion sort that roughly takes **c<sub>1</sub>n<sup>2</sup>** to sort n items where c<sub>1</sub> is a constant that does not depends on n. Now think about merge sort that takes **c<sub>2</sub>logn**, where c<sub>2</sub> here is another constant. Given an unsorted array of size n, we would like to sort it.

	n = 10000;
	Insertion sort takes 100 seconds;
	Merge sort takes 1.5 seconds; 

	n = 100000;
	Insertion sort takes ~3.25 hours;
	Merge sort takes less than 16 seconds; 

And that's a difference. I also have to mention that constants do not make big of a difference because what really maters is how many instructions (n) we are going to process, especially on larger inputs.

That's it for Chapter 1.
 {: style="text-align: left;"}

 
