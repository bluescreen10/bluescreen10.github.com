---
layout: post
title: Mavlink for Perl
---

A couple months ago I've started and implementation of [Mavlink][1] for Perl. Why? Because we're trying to build a [UAV][2] and we're using a [Beagleboard xM][3] as the brain, so given we've the processing power there was no need to suffer writing C. I'm not against it but I rather has the benefits of the rapid prototyping capabilities that scripting languages provide.

Luckily there are open source servers available: [QGroundControl][4] and [HappyKillmore][5] so we don't have to write that part, the idea is to build an event-driven framework that takes care of getting/sending messages to the Control Station and leave the user the implementation details of each one of the commands and actions. Once this project is finished building a Control Station in Perl should be possible and less difficult.

I'm also planning - if I've enough time - to build a bridge between Mavlink and  with [FlightGear][6] which is a flight simulator, with that I should be able to control planes in FlightGear through QGroundControl for instance and get real-time telemetry coming right from the simulator.

I started in TDD mode, but honestly I dropped it since I was spending a lot of time changing test each time I came up with a modification of the API. So I decide to define the API build a prototype test how that works from a user perspective and then continue on TDD.

I'm also giving [Mouse][7] a shoot, I didn't go for [Moose][8] because even this is an 1GHz processor I don't want to make my framework heavy weight from the start, although if Mouse is not enough I might give it a shot later on.

So far is on pre-alpha state located in [Github][9] and it may take some time to finish it up and making it CPAN ready. Once this is ready I hope this tempt other users to build flying drones using Perl.

To my knowledge this is the first attempt to put Perl code into a flying machine.

[1]: http://www.qgroundcontrol.org/mavlink/ "Mavlink"
[2]: http://en.wikipedia.org/wiki/Unmanned_aerial_vehicle "UAV"
[3]: http://beagleboard.org/hardware-xM "Beagleboard xM"
[4]: http://qgroundcontrol.org/ "QGroundControl"
[5]: http://code.google.com/p/happykillmore-gcs/downloads/list "HappyKillmore"
[6]: http://wiki.flightgear.org/Main_Page "FlightGear"
[7]: http://search.cpan.org/~gfuji/Mouse-0.93/lib/Mouse.pm "Mouse"
[8]: http://search.cpan.org/~doy/Moose-2.0010/lib/Moose.pm "Moose"
[9]: https://github.com/bluescreen10/Mavlink2 "Github"
