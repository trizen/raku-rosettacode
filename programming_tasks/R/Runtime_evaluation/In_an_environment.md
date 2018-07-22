[1]: https://rosettacode.org/wiki/Runtime_evaluation/In_an_environment

# [Runtime evaluation/In an environment][1]

For security, you must explicitly allow use of 'EVAL'.

```perl
use MONKEY-SEE-NO-EVAL;
sub eval_with_x($code, *@x) { [R-] @x.map: -> \x { EVAL $code } }
 
say eval_with_x('3 * x', 5, 10);      # Says "15".
say eval_with_x('3 * x', 5, 10, 50);  # Says "105".
```