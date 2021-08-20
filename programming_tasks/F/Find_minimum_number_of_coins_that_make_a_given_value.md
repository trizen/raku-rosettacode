[1]: https://rosettacode.org/wiki/Find_minimum_number_of_coins_that_make_a_given_value

# [Find minimum number of coins that make a given value][1]

Since unit denominations are possible, don't bother to check to see if an exact pay-out isn't possible.

```perl
my @denominations = 200, 100, 50, 20, 10, 5, 2, 1;
 
sub change (Int $n is copy where * >= 0) { gather for @denominations { take $n div $_; $n %= $_ } }
 
for 988, 1307, 37511, 0 -> $amount {
    say "\n$amount:";
    printf "%d × %d\n", |$_ for $amount.&change Z @denominations;
}
```

#### Output:
```
988:
4 × 200
1 × 100
1 × 50
1 × 20
1 × 10
1 × 5
1 × 2
1 × 1

1307:
6 × 200
1 × 100
0 × 50
0 × 20
0 × 10
1 × 5
1 × 2
0 × 1

37511:
187 × 200
1 × 100
0 × 50
0 × 20
1 × 10
0 × 5
0 × 2
1 × 1

0:
0 × 200
0 × 100
0 × 50
0 × 20
0 × 10
0 × 5
0 × 2
0 × 1
```
