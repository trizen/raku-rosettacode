[1]: http://rosettacode.org/wiki/Greatest_common_divisor

# [Greatest common divisor][1]

```perl6
sub gcd (Int $a is copy, Int $b is copy) {
   $a & $b == 0 and fail;
   ($a, $b) = ($b, $a % $b) while $b;
   return abs $a;
}
```
```perl6
multi gcd (0,      0)      { fail }
multi gcd (Int $a, 0)      { abs $a }
multi gcd (Int $a, Int $b) { gcd $b, $a % $b }
```
```perl6
my &gcd = { ($^a.abs, $^b.abs, * % * ... 0)[*-2] }
```
```perl6
my $gcd = $a gcd $b;
```


Because it's an infix, you can use it with various meta-operators:

```perl6
[gcd] @list;         # reduce with gcd
@alist Zgcd @blist;  # lazy zip with gcd
@alist Xgcd @blist;  # lazy cross with gcd
@alist »gcd« @blist; # parallel gcd
```