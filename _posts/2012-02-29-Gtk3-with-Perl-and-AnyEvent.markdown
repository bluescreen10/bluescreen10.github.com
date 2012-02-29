---
layout: post
title: Gtk3 with Perl and AnyEvent
---

Gtk+ its an awesome toolkit, cross platform, cross language it is all great but the documentation is terrible specially on scripting language's side. Basically if you want to get anything done you have to browse the official (yet incomplete), [StackOverflow][1] questions, blogs and even so you may not be able to find the answer.

Gtk3 is even more complicated because is relatively new and because most implementations use introspection to generate the bindings so there is no way to read the source of anything to see what methods or classes are available. I was looking for an example on how to draw a widget that can be updated based on an event, in other words, how to mix AnyEvent and Gtk3. It took me couple days to come up with a solution that works (Gtk3 changes things a little bit).

Here is the code...

{% highlight perl %}
#!/usr/bin/env perl
use warnings;
use strict;
use Cairo::GObject;
use Gtk3 '-init';
use AnyEvent;

my $window = Gtk3::Window->new('toplevel');
$window->signal_connect( 'destroy' => sub { Gtk3->main_quit } );

my $drawable = Gtk3::DrawingArea->new;
$drawable->signal_connect(
    'draw' => sub {
        my ( $widget, $context ) = @_;

        my $alloc = $widget->get_allocation;

        $context->move_to( rand( $alloc->{width} ), rand( $alloc->{height} ) );
        $context->rel_line_to( 10, 10 );
        $context->stroke;
    }
);

$window->add($drawable);
$window->show_all;

my $timer = AnyEvent->timer(
    interval => 0.5,
    cb       => sub { $drawable->queue_draw }
);

Gtk3->main;
{% endhighlight %} 

[1]:  http://stackoverflow.com/ "StackOverflow"
