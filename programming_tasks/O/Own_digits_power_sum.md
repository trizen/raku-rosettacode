[1]: https://rosettacode.org/wiki/Own_digits_power_sum

# [Own digits power sum][1]

```perl
(3..8).map: -> $p {
    my %pow = (^10).map: { $_ => $_ ** $p };
    my $start = 10 ** ($p - 1);
    my $end   = 10 ** $p;
    my @temp;
    for ^9 -> $i {
        ([X] ($i..9) xx $p).race.map: {
            next unless [<=] $_;
            my $sum = %pow{$_}.sum;
            next if $sum < $start;
            next if $sum > $end;
            @temp.push: $sum if $sum.comb.Bag eqv $_».Str.Bag
        }
    }
    .say for unique sort @temp;
}
```

#### Output:
```
153
370
371
407
1634
8208
9474
54748
92727
93084
548834
1741725
4210818
9800817
9926315
24678050
24678051
88593477
```


### Combinations with repetitions



Using code from [Combinations with repetitions](https://rosettacode.org/wiki/Combinations_with_repetitions) task, a version that runs relatively quickly, and scales well.

```perl
proto combs_with_rep (UInt, @ ) { * }
multi combs_with_rep (0,    @ ) { () }
multi combs_with_rep ($,    []) { () }
multi combs_with_rep (1,    @a) { map { $_, }, @a }
multi combs_with_rep ($n, [$head, *@tail]) {
    |combs_with_rep($n - 1, ($head, |@tail)).map({ $head, |@_ }),
    |combs_with_rep($n, @tail);
}

say sort gather {
    for 3..9 -> $d {
        for combs_with_rep($d, [^10]) -> @digits {
            .take if $d == .comb.elems and @digits.join == .comb.sort.join given sum @digits X** $d;
        }
    }
}
```

#### Output:
```
153 370 371 407 1634 8208 9474 54748 92727 93084 548834 1741725 4210818 9800817 9926315 24678050 24678051 88593477 146511208 472335975 534494836 912985153
```
