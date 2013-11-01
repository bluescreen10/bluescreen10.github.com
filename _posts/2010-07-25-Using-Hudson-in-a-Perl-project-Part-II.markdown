---
layout: post
title: Using Hudson in a Perl project - Part II
categories:
  - perl
  - continuous_integration
---

Let's get serious for a moment, This tutorial covers the basic steps to set up [Hudson continuous integration][1] for a [Perl][2] project but it doesn't cover it's installation, not only because it's a waste of time given the number of tutorials out there but also because it's really simple, in my case I've followed the instructions for Debian right from the link in Hudson's home page.

Assuming you have it installed with the default options you should be able to access to it by hitting [http://localhost:8080][3] in your browser.

![Main Screen][i1]

Now click in the *New Job* link located in the left part of the screen to launch the to Job's Creation Wizard, there we need to select job's name and job's type, in my case I've called it *MyProject* and for Perl projects the project's type is *free style software project*.

![Project Creation][i2]

Continuing now click *OK* to finish the project creation. Now it's time to start with project's setup, click on *Configure* to jump into the configuration screen, there you'll see a million options and I recommend you to click on the small icons with the question mark to bring the *quick-help* about them. First thing you need to configure is the source code repository, Hudson supports [CVS][4] and [SVN][5] out of the box but if you using [Git][6] or any other versioning system you can install plugins to use those.

As a sample project I'm going to use [Template::Toolkit][7] because author's repository is publicly available and it's a project with a considerable size. So in *Source Code Management* section select subversion and fill the [URL and module][8]

![SVN Settings][i3]

Builds can be triggered manually, scheduled, by another job or by changes in the repository. In this tutorial we will use the last option so we have to specify how often we want the repository to be polled for changes, in this case I've chosen *hourly*.

![SVN Polling Settings][i4]

Now the trickier part are the build steps, for Perl projects we'll use shell commands, so click on *Add build step* button and select *Execute shell* from the drop-down menu.

![Shell Command Settings][i5]

The following instructions may vary from project to project but the key part is that you have to ( at some point ) execute *prove* command. Hudson doesn't now anything about [TAP's][9] output but it understands [JUnit's][10] output, so we have to transform one into another that is done using [TAP::Formatter::JUnit][11].

![Shell Command Settings 2][i6]

The test results output is piped to *output.xml* file and that file is then picked up by Hudson after each build to determine the build's sanity, create charts and trigger other actions ( such as email notifications ). So in the Post build actions click on *Publish JUnit test result report* and specify the output file name *output.xml*.

![JUnit Output][i7]


That's it!, now click on *Save* at the bottom of the page. So, what about a test run??? Sure, why not? just click on *Build Now* ... and .... voila!

![Test results][i8]


As I said before Hudson has a lot of plugins and it's very flexible, for example, it's really simple to extend it to report test coverage using [Devel::Cover][12] or build [perldoc][13]> out of the [POD][14] included in the source files or run benchmark or profiling, the possibilities are unlimited it's just a matter of being creative and playing with the options.

Perl has a new allied and it's called Hudson!

[1]:  http://hudson-ci.org/ "Hudson"
[2]:  http://perl.org/ "Perl"
[3]:  http://localhost:8080/ "Hudson localhost"
[4]:  http://www.nongnu.org/cvs/ "CVS"
[5]:  http://subversion.apache.org/ "SVN"
[6]:  http://git-scm.com/ "Git"
[7]:  http://search.cpan.org/%7Eabw/Template-Toolkit-2.22/lib/Template/Toolkit.pod "Template::Toolkit"
[8]:  http://template-toolkit.org/download/index.html#svn "TT's SVN"
[9]:  http://en.wikipedia.org/wiki/Test_Anything_Protocol "TAP"
[10]: http://www.junit.org/ "JUnit"
[11]: http://search.cpan.org/%7Egtermars/TAP-Formatter-JUnit-0.08/lib/TAP/Formatter/JUnit.pm "Tap::Formater::JUnit"
[12]: http://search.cpan.org/%7Epjcj/Devel-Cover-0.67/lib/Devel/Cover.pm "Devel::Cover"
[13]: http://perldoc.org/ "perldoc"
[14]: http://en.wikipedia.org/wiki/Plain_Old_Documentation "POD"
[i1]: /images/posts/main_scree.png "Main Screen"
[i2]: /images/posts/project_creation.png "Project Creation Wizard"
[i3]: /images/posts/svn.png "SVN Settings"
[i4]: /images/posts/poll_svn.png "SVN Polling settings"
[i5]: /images/posts/execute_shell.png "Shell Command Settings"
[i6]: /images/posts/execute_shell_1.png "Shell Command Settings 2"
[i7]: /images/posts/junit.png "JUnit's output"
[i8]: /images/posts/results.png "Test results"