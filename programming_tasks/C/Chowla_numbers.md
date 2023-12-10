[1]: https://rosettacode.org/wiki/Chowla_numbers

# [Chowla numbers][1]


Much like in the [Totient function](https://rosettacode.org/wiki/Totient_function) task, we are using a thing poorly suited to finding prime numbers, to find large quantities of prime numbers.



(For a more reasonable test, reduce the orders-of-magnitude range in the "Primes count" line from 2..7 to 2..5)

```perl
sub comma { $^i.flip.comb(3).join(',').flip }

sub schnitzel (\Radda, \radDA = 0) {
    Radda.is-prime ?? !Radda !! ?radDA ?? Radda
    !! sum flat (2 .. Radda.sqrt.floor).map: -> \RAdda {
        my \RADDA = Radda div RAdda;
        next if RADDA * RAdda !== Radda;
        RAdda !== RADDA ?? (RAdda, RADDA) !! RADDA
    }
}

my \chowder = cache (1..Inf).hyper(:8degree).grep( !*.&schnitzel: 'panini' );

my \mung-daal = lazy gather for chowder -> \panini {
    my \gazpacho = 2**panini - 1;
    take gazpacho * 2**(panini - 1) unless schnitzel gazpacho, panini;
}

printf "chowla(%2d) = %2d\n", $_, .&schnitzel for 1..37;

say '';

printf "Count of primes up to %10s: %s\n", comma(10**$_),
  comma chowder.first( * > 10**$_, :k) for 2..7;

say "\nPerfect numbers less than 35,000,000";

.&comma.say for mung-daal[^5];
```

#### Output:
```
chowla( 1) =  0
chowla( 2) =  0
chowla( 3) =  0
chowla( 4) =  2
chowla( 5) =  0
chowla( 6) =  5
chowla( 7) =  0
chowla( 8) =  6
chowla( 9) =  3
chowla(10) =  7
chowla(11) =  0
chowla(12) = 15
chowla(13) =  0
chowla(14) =  9
chowla(15) =  8
chowla(16) = 14
chowla(17) =  0
chowla(18) = 20
chowla(19) =  0
chowla(20) = 21
chowla(21) = 10
chowla(22) = 13
chowla(23) =  0
chowla(24) = 35
chowla(25) =  5
chowla(26) = 15
chowla(27) = 12
chowla(28) = 27
chowla(29) =  0
chowla(30) = 41
chowla(31) =  0
chowla(32) = 30
chowla(33) = 14
chowla(34) = 19
chowla(35) = 12
chowla(36) = 54
chowla(37) =  0

Count of primes up to        100: 25
Count of primes up to      1,000: 168
Count of primes up to     10,000: 1,229
Count of primes up to    100,000: 9,592
Count of primes up to  1,000,000: 78,498
Count of primes up to 10,000,000: 664,579

Perfect numbers less than 35,000,000
6
28
496
8,128
33,550,336
```
