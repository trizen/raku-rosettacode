[1]: http://rosettacode.org/wiki/Exponentiation_order

# [Exponentiation order][1]

```perl6
sub demo($x) { say "  $x\t───► ", EVAL $x }
 
demo '5**3**2';      # show ** is right associative
demo '(5**3)**2';
demo '5**(3**2)';
 
demo '[**] 5,3,2';   # reduction form, show only final result
demo '[\**] 5,3,2';  # triangle reduction, show growing results
```

#### Output:
```
  5**3**2       ───► 1953125
  (5**3)**2     ───► 15625
  5**(3**2)     ───► 1953125
  [**] 5,3,2    ───► 1953125
  [\**] 5,3,2   ───► 2 9 1953125
```


Note that the reduction forms automatically go right-to-left because the base operator is right-associative. Most other operators are left-associative and would automatically reduce left-to-right instead.



While it is possible to define your own postfix operators to do exponentiation, Unicode does not have multilevel subscripts, and postfixes are always evaluated from inside out, so you can't stack them and expect right associativity:

```perl6
sub postfix:<²>($n) { $n * $n  }
sub postfix:<³>($n) { $n * $n * $n }
 
demo '(5³)²';
demo '5³²';
```

#### Output:
```
  (5³)² ───► 15625
  5³²   ───► 15625
```


(Not to mention the fact that the form without parentheses looks like you're trying to raise something to the 32nd power. Nor are you even allowed to parenthesize it the other way: <tt>5(³²)</tt> would be a syntax error. Despite all that, for programs that do a lot of squaring or cubing, the postfix forms can enhance both readability and concision.)