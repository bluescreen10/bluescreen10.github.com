---
layout: post
title: New language for Parrot VM
categories:
  - programming
  - parrot
---

During last year I've discovered the [Parrot VM][1] project, if you haven't heard about it, Parrot it's a register based VM targeted for dynamic languages.

Basically a VM is an abstraction layer between the Hardware/OS and a High Level Language(HLL) like [Java][2], [.Net][3], [Smalltalk][4], [Perl6][5] and others. The benefits are that same compiled code can run in different platforms without (almost) any modification - part of what made Java successful.

The difference between [stack-based VM][6] and [register-based VM][7] is that the first group push the parameters into the stack before calling a subroutine and the subroutine has to unwind the stack to read the parameters and wind it back with the results. That operation is performed a lot during a normal execution of a program wasting precious CPU cycles to wind/unwind the stack, if you want to read more about register-based and stack-based machines there is an interest paper [here][8].

The vision that the people from Parrot have is that anyone should be able to create a language and quickly implement it without worrying much about the VM, garbage collection, [JIT][9], etc. Also part of the vision is that different languages would be able to talk to each other and share libraries,What a small ideal, huh?

So I decide to give it a try and I started my own prototype-based language which is publicly available through [GitHub][10], it is called *mantra* ( I'm still not 100% convinced of the name ) and to give you an idea of how it feels like it is inspired in [Smalltalk][4], [Perl][11] and [Javascript][12] - wierd combination, I know. Basically I took things from each one of those languages that I'd liked, i.e. I think nothing beats Smalltalk in readability, so I've borrowed part of the syntax from there, or Perl's dynamism that everything can be changed in run time, or the fact that in Javascript you don't have classes and meta-classes.

There are many other cool things about it that I'll to share, but it is still at an embryonic level, meaning that is not yet usable and I'm still changing the specs. At this point I need a little more help with it, so if you willing to spend time in a cool project drop me an [email][13]

[1]:  http://www.parrot.org/ "Parrot VM"
[2]:  http://www.sun.com/java/ "Java"
[3]:  http://www.microsoft.com/net/ ".Net"
[4]:  http://en.wikipedia.org/wiki/Smalltalk "Smalltalk"
[5]:  http://perl6.org/ "Perl 6"
[6]:  http://en.wikipedia.org/wiki/Stack_machine "Stack Machine"
[7]:  http://en.wikipedia.org/wiki/Register_machine "Register Machine"
[8]:  http://www.usenix.org/events/vee05/full_papers/p153-yunhe.pdf "Register vs. Stack machines"
[9]:  http://en.wikipedia.org/wiki/Just-in-time_compilation "JIT"
[10]: http://github.com/bluescreen10/mantra "Mantra"
[11]: http://www.perl.org/ "Perl"
[12]: http://en.wikipedia.org/wiki/JavaScript "Javascript"
[13]: mailto:{{ site.email }} "email"