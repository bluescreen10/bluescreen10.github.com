---
layout: post
title: Using Hudson in a Perl project - Part I
categories:
  - perl
  - continuous_integration 
---

For those who doesn't know squat about [Hudson][1] it's a continuous integration server, maybe that doesn't tells you much but as you can see in its [page][2] it has two missions, one of them is build/test projects continuously.

[TDD][3] like methodologies state to run all test suite frequently but this isn't always the case, sometimes either developers run an small portion of the test suite or they forget to run it completely resulting in buggy builds. [Continuous Integration][4] (CI) servers contribute to reduce integration pains and ensure build's quality, specially in big or distributed development teams.

Hudson is quite simple to [install][5] and was originally created for Java projects but given its extensible nature it is possible to use it for non-java projects like Python, Ruby, C# or Perl. It has also cool features like test result charts, build time reports ( the time it takes to build and test a project ), test coverage charts, configurable actions each time a build fails and many other features.

In the next post I'll explain how to use Hudson in a Perl project! Stay tuned.

[1]: http://hudson-ci.org/ "Hudson"
[2]: http://wiki.hudson-ci.org/display/HUDSON/Meet+Hudson "Meet Hudson"
[3]: http://en.wikipedia.org/wiki/Test-driven_development "TDD"
[4]: http://en.wikipedia.org/wiki/Continuous_integration "Continous Integration"
[5]: http://wiki.hudson-ci.org/display/HUDSON/Installing+Hudson "Installing Hudson"