---
layout: post
title: Using Hudson in a Perl project - Part III
categories:
  - perl
  - continuous_integration
---

In this series of post I'll - this time - explain how to make Hudson create coverage reports for Perl projects, first of all you need to have [Devel::Cover][1] installed and Hudon's [Plot Plugin][2].

Basically to generate plots all you need is to create a file somewhere containing nothing but *YVALUE="XX.X"* after each build that's all [Plot Plugin][2] needs to generate charts.

We will use the following script to create such files I've named it cover_plot and upload it to the repository but it can be anywhere as long as Hudson can has access to it

{% highlight perl %}

#!/usr/bin/perl

use strict;

# Parse cover output
while (<STDIN>) {
    next if ( $_ !~ /^Total/ );
    chomp;

    /^Total[\t\s]
     +([\d\.\/na]+)[\t\s]
     +([\d\.\/na]+)[\t\s]
     +([\d\.\/na]+)[\t\s]
     +([\d\.\/na]+)[\t\s]
     +[\d\.\/na]+[\t\s]
     +([\d\.\/na]+)$/;

    my $statement_cover = $1;
    my $branch_cover    = $2;
    my $condition_cover = $3;
    my $sub_cover       = $4;
    my $all_cover       = $5;

    # Write file that PlotPlugin can understand
    write_file( 'coverage_stmt.properties', $statement_cover );
    write_file( 'coverage_bran.properties', $branch_cover );
    write_file( 'coverage_cond.properties', $condition_cover );
    write_file( 'coverage_sub.properties',  $sub_cover );
    write_file( 'coverage_all.properties',  $all_cover );
}

sub write_file {
    my ( $filename, $value ) = @_;
    my $file;

    open( $file, "> $filename" ) or die 'Can\'t open file';
    print $file "YVALUE=$value\n";
    close($file) or die 'Can\'t close file';
}

{% endhighlight %}

Then all we need to do is change Hudson's execute shell command and include the following

{% highlight bash %}

# Run tests and coverage
cover -delete
PERL5OPT=-MDevel::Cover=-ignore,.,-subs_only,on
          prove --formatter TAP::Formatter::JUnit -r t/ > build_test.xml

# Generate coverage html & plots
cover -silent -summary -outputdir coverage | ./hudson/cover_plot 

{% endhighlight %}

Now let's configure Hudson to pickup those files a create a chart with the values...

![Hudson's cover config][i1]

We're all set, now if you click on *Build now* and wait until it finishes you will discover a new button in the left bar called *Plots* and if you click there you should see something like

![Coverage Plot][i2]

Another tip: you can publish the full coverage report by configuring the *Publish documents* option in Hudson and pointing to the *coverage* folder.

Enjoy!

[1]:  http://search.cpan.org/%7Epjcj/Devel-Cover-0.73/lib/Devel/Cover.pm "Devel::Cover"
[2]:  http://wiki.hudson-ci.org/display/HUDSON/Plot+Plugin "Plot Plugin"
[i1]: /images/posts/hudson_config_cover.png "Hudson's cover config"
[i2]: /images/posts/hudson_coverage_plot.png "Coverage Plot"