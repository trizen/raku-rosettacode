[1]: https://rosettacode.org/wiki/Runtime_evaluation

# [Runtime evaluation][1]


Any syntactically valid sequence of statements may be run, and the snippet to be run can see its outer lexical scope at the point of the `eval`:

```perl
use MONKEY-SEE-NO-EVAL;

my ($a, $b) = (-5, 7);
my $ans = EVAL 'abs($a * $b)';  # => 35
```


Unlike in Perl 5, `eval` in Raku only compiles and executes the string, but does not trap exceptions.  You must say `try eval` to get that behavior (or supply a `CATCH` block within the text to be evaluated).
