[1]: http://rosettacode.org/wiki/Check_Machin-like_formulas

# [Check Machin-like formulas][1]

The task description requires exact computation. In this solution atan(a/b) produce floating points. Note that the Maxima solution is correct, since Maxima uses a symbolical representation of expressions.

```perl6
use Test;
plan *;
 
is tan(atan(1/2)+atan(1/3)), 1;
is tan(2*atan(1/3)+atan(1/7)), 1;
is tan(4*atan(1/5)-atan(1/239)), 1;
is tan(5*atan(1/7)+2*atan(3/79)), 1;
is tan(5*atan(29/278)+7*atan(3/79)), 1;
is tan(atan(1/2)+atan(1/5)+atan(1/8)), 1;
is tan(4*atan(1/5)-atan(1/70)+atan(1/99)), 1;
is tan(5*atan(1/7)+4*atan(1/53)+2*atan(1/4443)), 1;
is tan(6*atan(1/8)+2*atan(1/57)+atan(1/239)), 1;
is tan(8*atan(1/10)-atan(1/239)-4*atan(1/515)), 1;
is tan(12*atan(1/18)+8*atan(1/57)-5*atan(1/239)), 1;
is tan(16*atan(1/21)+3*atan(1/239)+4*atan(3/1042)), 1;
is tan(22*atan(1/28)+2*atan(1/443)-5*atan(1/1393)-10*atan(1/11018)), 1;
is tan(22*atan(1/38)+17*atan(7/601)+10*atan(7/8149)), 1;
is tan(44*atan(1/57)+7*atan(1/239)-12*atan(1/682)+24*atan(1/12943)), 1;
is tan(88*atan(1/172)+51*atan(1/239)+32*atan(1/682)+44*atan(1/5357)+68*atan(1/12943)), 1;
is tan(88*atan(1/172)+51*atan(1/239)+32*atan(1/682)+44*atan(1/5357)+68*atan(1/12944)), 1;
 
```

#### Output:
```
ok 1 - 
ok 2 - 
ok 3 - 
ok 4 - 
ok 5 - 
ok 6 - 
ok 7 - 
ok 8 - 
ok 9 - 
ok 10 - 
ok 11 - 
ok 12 - 
ok 13 - 
ok 14 - 
ok 15 - 
ok 16 - 
not ok 17 - 
#      got: '0.999999188225744'
# expected: '1'
```


The text input is almost directly parsable as Perl6 code, so we can just write:

```perl
use Test;
plan *;
 
for lines() {
    is tan(.[eval](http://perldoc.perl.org/functions/eval.html)), /INCORRECT/ ?? none(1) !! 1 given
    .subst(/'pi/4 = '/, '')\
    .subst(/arctan/, 'atan', :g)\
    .subst(/'[INCORRECT]'/, '# INCORRECT')
}
```


This program reads stdin.