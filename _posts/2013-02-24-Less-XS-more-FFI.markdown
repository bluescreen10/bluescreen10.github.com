---
layout: post
title: Less XS more FFI
categories:
  - perl
excerpt: Any successful new implementation of Perl5 will need to address the fact that 50% of CPAN modules use or depend on XS. The C-extensions are tied to the guts of current perl interpreter are by no means an small effort that the community will need to invest to port existent distributions over a new interpreter.
---

Times they are a-Changing in the Perl 5 community and there have been multiple riots in the past few months. Like [chromatic][1] pointed out it isn't the first time it happens but hopefully the last one. The hot topics are: [Pumpkin Perl][2], [Moe][3] and [Perl 11][4]. Although the three of them are different they all recognize that current state of affairs is not promising for the Perl 5 of the future.

Many said that [CPAN][5] is what keeps Perl 5 alive and I agree, let's face it, will you use a Perl 5 implementation that is 10 times better but doesn't run DBI? The answer is that you probably won't. Here is the deal, perl has [XS][6] and developers use it to: interface with an external library, speed things up, play with the perl guts or a combination of those three. The problem with XS is that it is tied to the current runtime and that around 50% of CPAN depend on XS. 

Any successful implementation of Perl will need to address the XS problem or have a lot of manpower. My hope is that the Perl community can agree on one FFI and API's to play with Perl's internals and slowly port modules using XS to these new interfaces, again lot of manpower by Perl's community itself and not an small group.

On the FFI side there have been many attempts, the one with more press was [Ctypes][7] but it is not complete nor is maintainable. Couple months ago I stumbled across [FFI::Raw][8] and fell in love with it, it's small, simple and does the Job. It isn't complete yet but has lots of potential. To prove my point I'm starting an small project to port [DateTime][9] from XS to FFI::Raw. I picked DateTime as an starting point because its usage of XS is small and very easy to convert.

So far I run into couple issues that I've reported to the author but made a lot of progress and some early micro-benchmarks show that it is at least 2 times faster, keep in mind that the idea here is not to speed up the runtime - which is a good thing - but to replace the need for XS.

I hope I can provide an alternative implementation of DateTime using FFI::Raw soon and prove the point that we can write less XS and start discussions on defining API's necessary for any alternative to be successful.

[1]: http://www.modernperlbooks.com/mt/2013/01/how-forking-perl-5-could-work.html "chromatic"
[2]: http://shadow.cat/blog/matt-s-trout/pumpkin-perl-breakdown/ "Pumpkin Perl"
[3]: https://github.com/MoeOrganization/moe "Moe"
[4]: http://perl11.org/ "Perl11"
[5]: https://metacpan.org/ "CPAN"
[6]: http://perldoc.perl.org/perlxs.html "XS"
[7]: http://gitorious.org/perl-ctypes "Ctypes"
[8]: https://metacpan.org/module/FFI::Raw "FFI::Raw"
[9]: https://metacpan.org/module/DateTime "DateTime"


