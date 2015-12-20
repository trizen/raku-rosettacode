[1]: http://rosettacode.org/wiki/String_case

# [String case][1]

In Perl 6, case modification is implemented as builtin subroutine or method:

```perl
my $word = "alpha BETA" ;
say [uc](http://perldoc.perl.org/functions/uc.html) $word;         # all uppercase (subroutine call)
say $word.[uc](http://perldoc.perl.org/functions/uc.html);         # all uppercase (method call)
# from now on we use only method calls as examples
say $word.[lc](http://perldoc.perl.org/functions/lc.html);         # all lowercase
say $word.tc;         # first letter titlecase
say $word.tclc;       # first letter titlecase, rest lowercase
say $word.tcuc;       # first letter titlecase, rest uppercase
say $word.wordcase;   # capitalize each word
 
```


Output:


#### Output:
```
ALPHA BETA
alpha beta
Alpha BETA
Alpha beta
ALPHA BETA
Alpha Beta
```