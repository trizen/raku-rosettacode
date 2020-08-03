[1]: https://rosettacode.org/wiki/String_case

# [String case][1]

In Perl 6, case modification is implemented as builtin subroutine or method:

```raku
my $word = "alpha BETA" ;
say uc $word;         # all uppercase (subroutine call)
say $word.uc;         # all uppercase (method call)
# from now on we use only method calls as examples
say $word.lc;         # all lowercase
say $word.tc;         # first letter titlecase
say $word.tclc;       # first letter titlecase, rest lowercase
say $word.wordcase;   # capitalize each word
 
```


Output:


#### Output:
```
ALPHA BETA
alpha beta
Alpha BETA
Alpha beta
Alpha Beta
```