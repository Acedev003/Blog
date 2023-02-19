---
layout: post
title: "Perspectives and Code - Redesigning Fibonacci"
description: 
tags: algorithms dynamic-programming perspectives-and-code
---

Among the first few coding challenges a newbie comes across is that of computing the Fibonacci Sequence. At a glance, this absurdly random question does not appear to offer any real world solutions, apart from the fact that it's used to demonstrate the concepts of recursion to new programmers.


A good use-case of the Fibonacci sequence that is not so popular is to explain concepts of algorithmic complexity. So, lets begin by looking at the definition of the so called sequence.


<div class="img_parent">
<img src="{{ "assets/images/2023-02-18-Redesigning-Fibonacci/fibonacci.png" | relative_url }}">
</div>

As the above image shows, every n'th element of the sequence is the sum of the previous 2 elements. This can represented by the formula 

<div class="img_parent">for n>1 :<br><b>Fib(n) = Fib(n-1) + Fib(n-2)</b></div>

Most of us who are familar with the concepts of recursion typically come up with a simple recursive algorithm to solve this problem

<div class="img_parent">
<img src="{{ "assets/images/2023-02-18-Redesigning-Fibonacci/fib1.png" | relative_url }}">
</div>

Now lets analyze the above solution. Lines 2 and 3 together take up O(1) time. Let T(n) represent the number of steps taken for the function <i>fib1(n)</i> to compute a result. The return statement at line 4 has a time complexity of T(n-1) + T(n-2). So overall, the time complexity of fib1 can be represented as follows:

<div class="img_parent"><b>T(n) &le; O(1) + T(n-1) + T(n-2)</b></div>

This formula looks similiar to that of the original definition of fibonacci numbers, except for the addition of a constant term O(1). Hence, it can concluded that the time complexity for n numbers is <u><i>atleast as large as the n'th Fibonacci number. </i></u>

<div class="img_parent"><b>T(n) &ge; Fib(n)</b></div>

We all know that fibonacci numbers grow exponentially in a constant &phi;, where &phi; equals (1+&radic;5)/2 which approximates to 1.61803 (aka the golden ratio). This is bad because our algorithm is now exponential in time complexity which makes it very slow. To put things into perspective lets plot the execution time of <i>fib1(n)</i> for increasing values of n.

<div class="img_parent">
<img src="{{ "assets/images/2023-02-18-Redesigning-Fibonacci/fib1plot.png" | relative_url }}">
</div>

As the plot shows, beyond a certain point the function takes a very large time to compute a result (approx 40 seconds to compute the value for <i>fib1(40)</i>). The reason behind these huge computation times can be visualized easily using the above tree which represents how the recursive algorithm works

<div class="img_parent">
<img src="{{ "assets/images/2023-02-18-Redesigning-Fibonacci/fibcalltree.gif" | relative_url }}">
</div>

It becomes evident from the above image that the solution for smaller subproblems gets computed more than once (eg: n=1 in the above tree), sometimes an exponential number of times. This is the root cause of the inefficiencies seen in this method of computing the fibonacci sequence. 


<hr>


What if we computed each sub-problem exactly once, and use previously computed values to arrive at the answer using a bottom-up approach. This programming methodology is known as <i><a href="https://en.wikipedia.org/wiki/Dynamic_programming">dynamic programming</a></i>. For our particular problem, we store the results for each n'th term in an array and use this array to compute the value for the (n+1)'th term.

<div class="img_parent">
<img src="{{ "assets/images/2023-02-18-Redesigning-Fibonacci/fibdp.gif" | relative_url }}">
</div>

Programming the above mentioned algorithm:

<div class="img_parent">
<img class="lg" src="{{ "assets/images/2023-02-18-Redesigning-Fibonacci/fib2.png" | relative_url }}">
</div>

We can see that the base case (lines 2-6) take up O(1) in time complexity. The for loop at lines 7-8 has a time complexity of O(n). Hence the new approach has a net time complexity of <b>approximately O(n)</b>. This is a huge improvement from the exponential time complexity seen in our first recursive implementation. Plotting the execution times for <i>fib2(n)</i> we get:

<div class="img_parent">
<img src="{{ "assets/images/2023-02-18-Redesigning-Fibonacci/fib2plot.png" | relative_url }}">
</div>

We can see an almost linear plot in the above figure. Also note that computing the 40th fibonacci number takes around 8x10<sup>-6</sup> seconds whereas the initial recursive implementation took 40 seconds for the same value of n, a very huge leap in performance.


This is a great example of how minor changes in perspective of how code is implemented can bring significant benefits in terms of performance.


<hr>

<small>
This post was inspired by Module-1 of Udacity's <a href="https://learn.udacity.com/courses/ud401">Intro to Graduate Algorithms Course</a>
</small>