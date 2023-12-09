[1]: https://rosettacode.org/wiki/Meissel–Mertens_constant

# [Meissel–Mertens constant][1]

```perl
# 20221011 Raku programming solution

my $s;
.is-prime and $s += log(1-1/$_)+1/$_ for 2 .. 10**8;
say $s + .57721566490153286
```

#### Output:
```
0.26149721310571383
```
