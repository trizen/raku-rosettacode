[1]: https://rosettacode.org/wiki/Partition_an_integer_x_into_n_primes

# [Partition an integer x into n primes][1]



```perl
use Math::Primesieve;
my $sieve = Math::Primesieve.new;

# short circuit for '1' partition
multi partition ( Int $number, 1 ) { $number.is-prime ?? $number !! () }

multi partition ( Int $number, Int $parts where * > 0 = 2 ) {
    my @these = $sieve.primes($number);
    for @these.combinations($parts) { .return if .sum == $number };
    ()
}

# TESTING
(18,2, 19,3, 20,4, 99807,1, 99809,1, 2017,24, 22699,1, 22699,2, 22699,3, 22699,4, 40355,3)\
  .race(:1batch).map: -> $number, $parts {
    say (sprintf "Partition %5d into %2d prime piece", $number, $parts),
    $parts == 1 ?? ':  ' !! 's: ', join '+', partition($number, $parts) || 'not possible'
}
```

#### Output:
```
Partition    18 into  2 prime pieces: 5+13
Partition    19 into  3 prime pieces: 3+5+11
Partition    20 into  4 prime pieces: not possible
Partition 99807 into  1 prime piece:  not possible
Partition 99809 into  1 prime piece:  99809
Partition  2017 into 24 prime pieces: 2+3+5+7+11+13+17+19+23+29+31+37+41+43+47+53+59+61+67+71+73+79+97+1129
Partition 22699 into  1 prime piece:  22699
Partition 22699 into  2 prime pieces: 2+22697
Partition 22699 into  3 prime pieces: 3+5+22691
Partition 22699 into  4 prime pieces: 2+3+43+22651
Partition 40355 into  3 prime pieces: 3+139+40213
```
