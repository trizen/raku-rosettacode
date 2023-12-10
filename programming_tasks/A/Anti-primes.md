[1]: https://rosettacode.org/wiki/Anti-primes

# [Anti-primes][1]





At its heart, this task is almost exactly the same as [Proper_Divisors](https://rosettacode.org/wiki/Proper_Divisors), it is just asking for slightly different results. Much of this code is lifted straight from there.



Implemented as an auto-extending lazy list. Displaying the count of anti-primes less than 5e5 also because... why not.

```perl
sub propdiv (\x) {
    my @l = 1 if x > 1;
    (2 .. x.sqrt.floor).map: -> \d {
        unless x % d { @l.push: d; my \y = x div d; @l.push: y if y != d }
    }
    @l
}

my $last = 0;

my @anti-primes = lazy 1, |(|(2..59), 60, *+60 … *).grep: -> $c {
    my \mx = +propdiv($c);
    next if mx <= $last;
    $last = mx;
    $c
}

my $upto = 5e5;

put "First 20 anti-primes:\n{ @anti-primes[^20] }";

put "\nCount of anti-primes <= $upto: {+@anti-primes[^(@anti-primes.first: * > $upto, :k)]}";
```

#### Output:
```
First 20 anti-primes:
1 2 4 6 12 24 36 48 60 120 180 240 360 720 840 1260 1680 2520 5040 7560

Count of anti-primes <= 500000: 35
```
