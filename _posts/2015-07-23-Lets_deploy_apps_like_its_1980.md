---
layout: post
title: Let's deploy applications like it's 1980
categories:
  - thoughts
---

It's 1980, the world of computing is dominated by big blue, personal computers are just in the early days and a little project out of a research lab was just released to the public.

[Smalltalk-80][1] was released as a combination of a platform independent image and a vm specification. Among it's many innovations incorporated a graphical user interface based on a software abstraction called MVC.

At the same time multi-user applications are deployed to big servers capable of handling the load. Sometimes packing several applications within the same server to utilize resources (CPU, Memory and Storage) more efficiently.

Fast forward 35 years to year 2015. Personal computers are everywhere and in rich countries people even have more than one personal computing device. A cigarette box sized $35 dollar computer is orders of magnitude faster than the fastest computer back in 1980. A single 64GB SD card is capable of storing the data produced by thousands of companies back in the 80s.

A very different world in a lot of ways...

The computing industry evolved to join multiple relatively small and cheap computers into a cluster so a centralize controller manage all resources provided by these servers as big pool available to applications. The field also discovered that it is better to give developers tools that allow them define the execution context of their applications so they can run on any computers that conform to a common standard.

In the year 2015 the state-of-the-art is to deploy several applications as platform independent images ([Docker][2]) on (virtual) big servers ([Apache Mesos][3]) capable of handling the load.

Can't help to think Déjà Vu!

[1]: https://en.wikipedia.org/wiki/Smalltalk
[2]: https://www.docker.com/
[3]: http://mesos.apache.org/
