[1]: https://rosettacode.org/wiki/Mertens_function

# [Mertens function][1]





Mertens number is not defined for n == 0. Raku arrays are indexed from 0 so store a blank value at position zero to keep x and M(x) aligned.

```perl
use Prime::Factor;

sub μ (Int \n) {
    return 0 if n %% 4 or n %% 9 or n %% 25 or n %% 49 or n %% 121;
    my @p = prime-factors(n);
    +@p == +@p.unique ?? +@p %% 2 ?? 1 !! -1 !! 0
}

my @mertens = lazy [\+] flat '', 1, (2..*).hyper.map: -> \n { μ(n) };

put "Mertens sequence - First 199 terms:\n",
    @mertens[^200]».fmt('%3s').batch(20).join("\n"),
    "\n\nEquals zero ", +@mertens[1..1000].grep( !* ),
    ' times between 1 and 1000', "\n\nCrosses zero ",
    +@mertens[1..1000].kv.grep( {!$^v and @mertens[$^k]} ),
    " times between 1 and 1000\n\nFirst Mertens equal to:";

for 10, 20, 30 … 100 -> $threshold {
    printf "%4d: M(%d)\n", -$threshold, @mertens.first: * == -$threshold, :k;
    printf "%4d: M(%d)\n",  $threshold, @mertens.first: * ==  $threshold, :k;
}
```

#### Output:
```
Mertens sequence - First 199 terms:
      1   0  -1  -1  -2  -1  -2  -2  -2  -1  -2  -2  -3  -2  -1  -1  -2  -2  -3
 -3  -2  -1  -2  -2  -2  -1  -1  -1  -2  -3  -4  -4  -3  -2  -1  -1  -2  -1   0
  0  -1  -2  -3  -3  -3  -2  -3  -3  -3  -3  -2  -2  -3  -3  -2  -2  -1   0  -1
 -1  -2  -1  -1  -1   0  -1  -2  -2  -1  -2  -3  -3  -4  -3  -3  -3  -2  -3  -4
 -4  -4  -3  -4  -4  -3  -2  -1  -1  -2  -2  -1  -1   0   1   2   2   1   1   1
  1   0  -1  -2  -2  -3  -2  -3  -3  -4  -5  -4  -4  -5  -6  -5  -5  -5  -4  -3
 -3  -3  -2  -1  -1  -1  -1  -2  -2  -1  -2  -3  -3  -2  -1  -1  -1  -2  -3  -4
 -4  -3  -2  -1  -1   0   1   1   1   0   0  -1  -1  -1  -2  -1  -1  -2  -1   0
  0   1   1   0   0  -1   0  -1  -1  -1  -2  -2  -2  -3  -4  -4  -4  -3  -2  -3
 -3  -4  -5  -4  -4  -3  -4  -3  -3  -3  -4  -5  -5  -6  -5  -6  -6  -7  -7  -8

Equals zero 92 times between 1 and 1000

Crosses zero 59 times between 1 and 1000

First Mertens equal to:
 -10: M(659)
  10: M(1393)
 -20: M(2791)
  20: M(3277)
 -30: M(9717)
  30: M(8503)
 -40: M(9831)
  40: M(11770)
 -50: M(24018)
  50: M(19119)
 -60: M(24105)
  60: M(31841)
 -70: M(24170)
  70: M(31962)
 -80: M(42789)
  80: M(48202)
 -90: M(59026)
  90: M(48405)
-100: M(59426)
 100: M(114717)
```
