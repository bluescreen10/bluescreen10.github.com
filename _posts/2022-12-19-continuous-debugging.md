---
layout: post
title: Continuous Debugging
---
In the age of fancy machine learning models with billions of parameters that can *almost* drive cars, or answer complex questions, or even write code. By far the most common way of debugging a program is by inserting print statements in the code. so much so that I'd argue that most programmers don't know how to spin or use a debugger.

For most industries technology has brought better tools. Allowing people be more productive. Yet programmers still use a text editor and the same techniques they have used for decades. There are though some exceptions such as [glamourous toolkit](https://gtoolkit.com/) but radical ideas like that are not mainstream - yet. We can do better, we *must* do better.

For any non-trivial system, when things go wrong, there are probably multiple explanations as to why. The typically flow to find the cause is: come up with a hypothesis, add a print line to the code to confirm or reject it, re-run the program and get it to fail again. Hopefully this will confirm the hypothesis but if not pick a new one and do it all over again. This is a waste of lots of human cycles.

Alternatively you could run your program in the debugger. Even if you are not trying to find a bug. And when invariably something goes wrong, you could go to your debug console, add some breakpoints and without modifying the program figure out what's causing the problem. I call this technique *continuous debugging* and it's a programmer's superpower.
