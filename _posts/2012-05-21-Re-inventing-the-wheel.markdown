---
layout: post
title: Re-inventing the Wheel 
---

I'm working in a communication protocol between a custom hardware and an application to configure that device. Unfortunately this device lacks of a TCP/IP stack so I'm basically forced to work through a serial interface.

This means that things that we take for granted in modern development have to be built once again. Things like dealing with buffers sizes, communication errors and checksums. While this is fun and something I encourage that you should at least do once in your career it gets boring once you do it over and over.

All protocols I've implemented are almost similar they have header, packet type, length, payload, checksum but they are slightly different that makes code not reusable. In short I couldn't come up with an abstraction that works for all protocols and at the same time is conceptually simple.

I wish it was a way to define a template for structure messages (length, type, endianness ). If I stumble once again doing the same thing I may write it myself.
