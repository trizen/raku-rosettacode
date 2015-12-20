[1]: http://rosettacode.org/wiki/Arithmetic/Complex

# [Arithmetic/Complex][1]

```perl6
my $a = 1 + i;
my $b = pi + 1.25i;
 
.say for $a + $b, $a * $b, -$a, 1 / $a, $a.conj;
.say for $a.abs, $a.sqrt, $a.re, $a.im;
```

#### Output:
```
4.1415926535897931+2.25i
1.8915926535897931+4.3915926535897931i
-1-1i
0.5-0.5i
1-1i
1.4142135623730951
1.0986841134678098+0.45508986056222733i
1
1
```