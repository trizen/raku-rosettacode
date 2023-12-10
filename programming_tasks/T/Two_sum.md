[1]: https://rosettacode.org/wiki/Two_sum

# [Two sum][1]





### Procedural

```perl
sub two_sum ( @numbers, $sum ) {
    die '@numbers is not sorted' unless [<=] @numbers;

    my ( $i, $j ) = 0, @numbers.end;
    while $i < $j {
        given $sum <=> @numbers[$i,$j].sum {
            when Order::More { $i += 1 }
            when Order::Less { $j -= 1 }
            when Order::Same { return $i, $j }
        }
    }
    return;
}

say two_sum ( 0, 2, 11, 19, 90 ), 21;
say two_sum ( 0, 2, 11, 19, 90 ), 25;
```

#### Output:
```
(1 3)
Nil
```


### Functional



The two versions differ only in how one 'reads' the notional flow of processing: left-to-right versus right-to-left.
Both return all pairs that sum to the target value, not just the first (e.g. for input of `0 2 10 11 19 90` would get indices 1/4 and 2/3).

```perl
sub two-sum-lr (@a, $sum) {
  # (((^@a X ^@a) Z=> (@a X+ @a)).grep($sum == *.value)>>.keys.map:{ .split(' ').sort.join(' ')}).unique
    (
     (
      (^@a X ^@a) Z=> (@a X+ @a)
     ).grep($sum == *.value)>>.keys
     .map:{ .split(' ').sort.join(' ')}
    ).unique
}

sub two-sum-rl (@a, $sum) {
  # unique map {.split(' ').sort.join(' ')}, keys %(grep {.value == $sum}, ((^@a X ^@a) Z=> (@a X+ @a)))
    unique
    map {.split(' ').sort.join(' ')},
    keys %(
     grep {.value == $sum}, (
      (^@a X ^@a) Z=> (@a X+ @a)
     )
    )
}

my @a = <0 2 11 19 90>;
for 21, 25 {
    say two-sum-rl(@a, $_);
    say two-sum-lr(@a, $_);
}
```

#### Output:
```
(1 3)
(1 3)
()
()
```
