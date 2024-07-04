[1]: https://rosettacode.org/wiki/Greatest_prime_dividing_the_n-th_cubefree_number

# [Greatest prime dividing the n-th cubefree number][1]

```perl
use Prime::Factor;

sub max_factor_if_cubefree ($i) {
    my @f = prime-factors($i);
    return @f.tail if @f.elems          < 3
                   or @f.Bag.values.all < 3;
}

constant @Aₙ = lazy flat 1, map &max_factor_if_cubefree, 2..*;

say 'The first terms of A370833 are:';
say .fmt('%3d') for @Aₙ.head(100).batch(10);

say '';

for 10 X** (3..6) -> $k {
    printf "The %8dth term of A370833 is %7d\n", $k, @Aₙ[$k-1];
}
```

#### Output:
```
The first terms of A370833 are:
  1   2   3   2   5   3   7   3   5  11
  3  13   7   5  17   3  19   5   7  11
 23   5  13   7  29   5  31  11  17   7
  3  37  19  13  41   7  43  11   5  23
 47   7   5  17  13  53  11  19  29  59
  5  61  31   7  13  11  67  17  23   7
 71  73  37   5  19  11  13  79  41  83
  7  17  43  29  89   5  13  23  31  47
 19  97   7  11   5 101  17 103   7  53
107 109  11  37 113  19  23  29  13  59

The     1000th term of A370833 is     109
The    10000th term of A370833 is     101
The   100000th term of A370833 is    1693
The  1000000th term of A370833 is 1202057
```
