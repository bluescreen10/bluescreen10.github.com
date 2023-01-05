---
layout: default
title: Mocks are for schmocks
---

This is a rant about the use of libraries such as [MagicMock](https://docs.python.org/3/library/unittest.mock.html#unittest.mock.MagicMock) or [go-sqlmock](https://github.com/DATA-DOG/go-sqlmock) for writing tests. I really believe that you should avoid any library that matches the following regular expression `/(mock|patch)/`.

Tests should be short, concise, only a few lines long. When you use mocks the test tends to be dominated by set-up code. To the point that's difficult to understand what is going on and what the test is about.

For some libraries you need to specify the expected calls and the order of those. To do that, you need to know how a particular function or method is implemented, creating a tight couple between the test and the structure of the code under test. Tests should assert outputs irrespective of how things are wired internally.

Since they replace a real dependency, every time the dependency changes you need to update your mocks. In large codebases with lots of tests this can be really costly.

I guess my biggest complaint about using this technique, is that it doesn't guarantee your code will run with real dependencies. It can still fail miserably, for instance, if you have a typo in your SQL query.

Next time you think you need to use mocks, take it a sign that you might have a design problem with your code.
