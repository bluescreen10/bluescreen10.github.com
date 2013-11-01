---
layout: post
title: If you don't need to then don't do it
categories:
  - perl
  - programming
---

Recently I've submitted a patch for one of the CPAN modules, the module connected to a server to make sure the server its alive before sending any command to it.

Let me sketch the code:

{% highlight perl %}

sub new {
    ...
    eval {
         $self->test_connection($target_host);         
    };

    if ($@) {
       die "Can't Connect";
    }
}

{% endhighlight %} 

The problem is even more complex, because depending on the server the test connection message could return 200 as response code or 500. I was trying to point out that the extra step was completely unnecessary, the author was pointing out that just because the server answers something means that is alive, that's true, but *what value does it brings?*, *Does that means the it will be able to process commands?* ehrr.. I don't know, *Does it means I'm sure the server will be available when I start sending commands to it?* I can't tell.

The former approach has other downsides: 

 1. Let's assume that server changes the behavior and instead of returning error 500 returns 404, then our code won't caught that and it will be broken because our code is expecting 200 or 500.
 2. Now let's say we create an object but we don't use it, with this code since the **test_connection** is part of the **new** method that means it will be executed always even if then we don't use the object.
 3. We have extra overhead because instead of sending our first command when we need it is always sending a "ping" before

What will happen if the server responds the first time and then it crashes? But... it was working a minute ago... so????

In my opinion it's better to limit the number of times you call an external resource and call it only when you need them.

