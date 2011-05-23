---
layout: post
title: Exposing Web Services in Perl
---

I've been looking for a good framework in [Perl][1] to write [Web Services][2] and it looks like there isn't. I believe the two main reasons for that is because Perl's [Weak Typing][3] nature and lack of out of the box subroutine signature, this means that the compiler doesn't know at compile time what parameters and which type does a subroutine needs. While in one hand this give you a flexible language in the other hand it makes almost impossible any attempt of meta-descriptions.

The traditional approach to write a Web Service in Perl is to write the backend to attend client's petitions and the meta-descriptive documents ( [XSD][4] and [WSDL][5]). Now imagine you have a Web Service and you want to turn it into a [JSON-RPC][6] service, this means that part of your backend has to be re-written - at least the parser - also you have to write new meta-descriptive documents in this case [SMD][7]. To go a little bit further imagine if you want to have both, this is more likely to create a maintenance hell, any change in one piece may trigger changes in all other pieces and if you want to continue offering different faces ( WS, JSON-RPC, Rest, ... ) this becomes a task for a team of highly trained monkeys.

[Strong Typed][8] languages like [Java][9] have fancy frameworks that help you build WS in seconds through the use of [Annotations][10] also something that Perl lacks of. Those frameworks take care of creating the baking meta-descriptive documents, this is very productive as you don't have to create/maintain them, any changes you make to the backend is immediately reflected in service description documents.

Fortunately Perl subroutines have [attributes][11] which is a sort of annotations flexible enough if you keep a minimal syntax. Attributes can be used to describe for example the subroutine's parameters and their data types and/or the return value, that information should be sufficient to fill the gap and bring easy to create/maintain services into Perl.

That's something I'm trying to hack in a project called "Domuyo" which intends to be a framework for creating web services easily in Perl, to give you a feeling how a controller class would look like here's a snippet:

{% highlight perl %}

package MyController;

use strict;
use warnings;
use base qw(Domuyo::Controller);

sub add :METHOD(Int a, Int b) RETURNS(Int) {
    my ($self, %args) = @_;
    return $args{a} + $args{b}
}

{% endhighlight %} 

And the framework would be responsible of creating the XSD, WSDL, SMD or any other meta-descriptive document as well as parse the request and dispatch it. At this moment I've parts of the skeleton of the framework and made a couple of proof of concepts scripts and as soon as I've something to release I will definitely do. Don't switch the channel.

[1]:  href="http://www.perl.org/ "Perl"
[2]:  http://en.wikipedia.org/wiki/Web_service "Web Service"
[3]:  http://en.wikipedia.org/wiki/Weak_typing "Weak Typing"
[4]:  http://www.w3.org/TR/xmlschema-0/ "XSD"
[5]:  http://www.w3.org/TR/wsdl "WSDL"
[6]:  http://en.wikipedia.org/wiki/JSON-RPC "JSON-RPC"
[7]:  http://groups.google.com/group/json-schema/web/service-mapping-description-proposal "SMD"
[8]:  http://en.wikipedia.org/wiki/Strong_typing "Strong Typed"
[9]:  http://www.java.com/en/ "Java"
[10]: http://en.wikipedia.org/wiki/Java_annotation "Annontations"
[11]: http://perldoc.perl.org/attributes.html "Attributes"
