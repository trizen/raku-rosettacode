[1]: https://rosettacode.org/wiki/Exponentiation_order

# [Exponentiation order][1]

Note that the reduction forms automatically go right-to-left because the base operator is right-associative. Most other operators are left-associative and would automatically reduce left-to-right instead.

```perl
use MONKEY-SEE-NO-EVAL;
sub demo($x) { say "  $x\t───► ", EVAL $x }
 
demo '5**3**2';      # show ** is right associative
demo '(5**3)**2';
demo '5**(3**2)';
 
demo '[**] 5,3,2';   # reduction form, show only final result
demo '[\**] 5,3,2';  # triangle reduction, show growing results
 
# Unicode postfix exponents are supported as well:
 
demo '(5³)²';
demo '5³²';
 
```

#### Output:
```
  5**3**2       ───► 1953125
  (5**3)**2     ───► 15625
  5**(3**2)     ───► 1953125
  [**] 5,3,2    ───► 1953125
  [\**] 5,3,2   ───► 2 9 1953125
  (5³)² ───► 15625
  5³²   ───► 23283064365386962890625
```


The Unicode exponent form without parentheses ends up raising to the 32nd power. Nor are you even allowed to parenthesize it the other way: `5(³²)` would be a syntax error. Despite all that, for programs that do a lot of squaring or cubing, the postfix forms can enhance both readability and concision.