[1]: https://rosettacode.org/wiki/Greatest_common_divisor

# [Greatest common divisor][1]

### Iterative

```raku
sub gcd (Int $a is copy, Int $b is copy) {
   $a & $b == 0 and fail;
   ($a, $b) = ($b, $a % $b) while $b;
   return abs $a;
}
```


### Recursive

```raku
multi gcd (0,      0)      { fail }
multi gcd (Int $a, 0)      { abs $a }
multi gcd (Int $a, Int $b) { gcd $b, $a % $b }
```


### Concise

```raku
my &gcd = { ($^a.abs, $^b.abs, * % * ... 0)[*-2] }
```


### Actually, it's a built-in infix

```raku
my $gcd = $a gcd $b;
```


Because it's an infix, you can use it with various meta-operators:

```raku
[gcd] @list;         # reduce with gcd
@alist Zgcd @blist;  # lazy zip with gcd
@alist Xgcd @blist;  # lazy cross with gcd
@alist »gcd« @blist; # parallel gcd
```