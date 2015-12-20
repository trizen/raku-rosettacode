[1]: http://rosettacode.org/wiki/Arithmetic/Rational

# [Arithmetic/Rational][1]

Perl 6 supports rational arithmetic natively.

```perl
for 2..2**19 -> $candidate {
    my $sum = 1 / $candidate;
    for 2 .. ceiling(sqrt($candidate)) -> $factor {
        if $candidate %% $factor {
            $sum += 1 / $factor + 1 / ($candidate / $factor);
        }
    }
    if $sum.denominator == 1 {
        say "Sum of reciprocal factors of $candidate = $sum exactly", ($sum == 1 ?? ", perfect!" !! ".");
    }
}
```


Note also that ordinary decimal literals are stored as Rats, so the following loop always stops exactly on 10 despite 0.1 not being exactly representable in floating point:

```perl
for 1.0, 1.1, 1.2 ... 10 { .say }
```


The arithmetic is all done in rationals, which are converted to floating-point just before display so that people don't have to puzzle out what 53/10 means.