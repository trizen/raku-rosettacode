[1]: https://rosettacode.org/wiki/Magic_numbers

# [Magic numbers][1]

```perl
my \Δ = $ = 1;
my @magic = flat 0, [1..9], {last if .not; ++Δ; [(.flat X~ 0..9).grep: * %% Δ]}…*;

put "There are {@magic.eager.elems} magic numbers in total.";
put "\nThe largest is {@magic.tail}.";
put "\nThere are:";
put "{(+.value).fmt: "%4d"} with {.key.fmt: "%2d"} digit{1 == +.key ?? '' !! 's'}"
    for sort @magic.classify: {.chars};
{
    my $pan-digital = ($_..9).join.comb.Bag;
    put "\nAll magic numbers that are pan-digital in $_ through 9 with no repeats: " ~
    @magic.grep( { .comb.Bag eqv $pan-digital } );
} for 1, 0;
```

#### Output:
```
There are 20457 magic numbers in total.

The largest is 3608528850368400786036725.

There are:
  10 with  1 digit
  45 with  2 digits
 150 with  3 digits
 375 with  4 digits
 750 with  5 digits
1200 with  6 digits
1713 with  7 digits
2227 with  8 digits
2492 with  9 digits
2492 with 10 digits
2225 with 11 digits
2041 with 12 digits
1575 with 13 digits
1132 with 14 digits
 770 with 15 digits
 571 with 16 digits
 335 with 17 digits
 180 with 18 digits
  90 with 19 digits
  44 with 20 digits
  18 with 21 digits
  12 with 22 digits
   6 with 23 digits
   3 with 24 digits
   1 with 25 digits

All magic numbers that are pan-digital in 1 through 9 with no repeats: 381654729

All magic numbers that are pan-digital in 0 through 9 with no repeats: 3816547290
```
