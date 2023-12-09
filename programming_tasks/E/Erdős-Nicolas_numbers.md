[1]: https://rosettacode.org/wiki/Erdős-Nicolas_numbers

# [Erdős-Nicolas numbers][1]

```perl
use Prime::Factor;

sub is-Erdős-Nicolas ($n) {
    my @divisors = $n.&proper-divisors: :s;
    ((@divisors.sum > $n) && (my $key = ([\+] @divisors).first: $n, :k)) ?? 1 + $key !! False
}

my $count;

(1..*).hyper(:2000batch).map( * × 2 ).map: {
    if my $key = .&is-Erdős-Nicolas {
        printf "%8d == sum of its first %3d divisors\n", $_, $key;
        exit if ++$count >= 8;
    }
}
```

#### Output:
```
      24 == sum of its first   6 divisors
    2016 == sum of its first  31 divisors
    8190 == sum of its first  43 divisors
   42336 == sum of its first  66 divisors
   45864 == sum of its first  66 divisors
  392448 == sum of its first  68 divisors
  714240 == sum of its first 113 divisors
 1571328 == sum of its first 115 divisors
```
