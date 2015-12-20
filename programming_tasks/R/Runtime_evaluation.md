[1]: http://rosettacode.org/wiki/Runtime_evaluation

# [Runtime evaluation][1]

Any syntactically valid sequence of statements may be run, and the snippet to be run can see its outer lexical scope at the point of the <tt>eval</tt>:

```perl
my ($a, $b) = (-5, 7);
my $ans = [eval](http://perldoc.perl.org/functions/eval.html) 'abs($a * $b)';  # => 35
```


Unlike in Perl 5, <tt>eval</tt> in Perl 6 only compiles and executes the string, but does not trap exceptions. You must say <tt>try eval</tt> to get that behavior (or supply a <tt>CATCH</tt> block within the text to be evaluated).