[1]: https://rosettacode.org/wiki/Semiprime

# [Semiprime][1]

Here is a naive, grossly inefficient implementation.

```perl
sub is-semiprime (Int $n --> Bool) {
    not $n.is-prime and
        .is-prime given 
        $n div first $n %% *, flat grep &is-prime, 2 .. *;
}
 
use Test;
my @primes = flat grep &is-prime, 2 .. 100;
for ^5 {
    nok is-semiprime([*] my @f1 = @primes.roll(1)), ~@f1;
    ok  is-semiprime([*] my @f2 = @primes.roll(2)), ~@f2;
    nok is-semiprime([*] my @f3 = @primes.roll(3)), ~@f3;
    nok is-semiprime([*] my @f4 = @primes.roll(4)), ~@f4;
}
```

#### Output:
```
ok 1 - 17
ok 2 - 47 23
ok 3 - 23 37 41
ok 4 - 53 37 67 47
ok 5 - 5
ok 6 - 73 43
ok 7 - 13 53 71
ok 8 - 7 79 37 71
ok 9 - 41
ok 10 - 71 37
ok 11 - 37 53 43
ok 12 - 3 2 47 67
ok 13 - 17
ok 14 - 41 61
ok 15 - 71 31 79
ok 16 - 97 17 73 17
ok 17 - 61
ok 18 - 73 47
ok 19 - 13 19 5
ok 20 - 37 97 11 31
```


### More efficient example



Here is a more verbose, but MUCH more efficient implementation. Demonstrating using it to find an infinite list of semiprimes and to check a range of integers to find the semiprimes.

```perl
sub is-semiprime ( Int $n where * > 0 ) {
    return False if $n.is-prime;
    my $factor = find-factor( $n );
    return True if $factor.is-prime && ( $n div $factor ).is-prime;
    False;
}
 
sub find-factor ( Int $n, $constant = 1 ) {
    my $x      = 2;
    my $rho    = 1;
    my $factor = 1;
    while $factor == 1 {
        $rho *= 2;
        my $fixed = $x;
        for ^$rho {
            $x = ( $x * $x + $constant ) % $n;
            $factor = ( $x - $fixed ) gcd $n;
            last if 1 < $factor;
        }
    }
    $factor = find-factor( $n, $constant + 1 ) if $n == $factor;
    $factor;
}
 
INIT my $start = now;
 
# Infinite list of semiprimes
constant @semiprimes = lazy gather for 4 .. * { .take if .&is-semiprime };
 
# Show the semiprimes < 100
say 'Semiprimes less than 100:';
say @semiprimes[^ @semiprimes.first: * > 100, :k ], "\n";
 
# Check individual integers, or in this case, a range
my $s = 2⁹⁷ - 1;
say "Is $_ semiprime?: ", .&is-semiprime for $s .. $s + 30;
 
say 'elapsed seconds: ', now - $start;
 
```

#### Output:
```
Semiprimes less than 100:
(4 6 9 10 14 15 21 22 25 26 33 34 35 38 39 46 49 51 55 57 58 62 65 69 74 77 82 85 86 87 91 93 94 95)

Is 158456325028528675187087900671 semiprime?: True
Is 158456325028528675187087900672 semiprime?: False
Is 158456325028528675187087900673 semiprime?: False
Is 158456325028528675187087900674 semiprime?: False
Is 158456325028528675187087900675 semiprime?: False
Is 158456325028528675187087900676 semiprime?: False
Is 158456325028528675187087900677 semiprime?: False
Is 158456325028528675187087900678 semiprime?: False
Is 158456325028528675187087900679 semiprime?: False
Is 158456325028528675187087900680 semiprime?: False
Is 158456325028528675187087900681 semiprime?: False
Is 158456325028528675187087900682 semiprime?: False
Is 158456325028528675187087900683 semiprime?: False
Is 158456325028528675187087900684 semiprime?: False
Is 158456325028528675187087900685 semiprime?: False
Is 158456325028528675187087900686 semiprime?: False
Is 158456325028528675187087900687 semiprime?: False
Is 158456325028528675187087900688 semiprime?: False
Is 158456325028528675187087900689 semiprime?: False
Is 158456325028528675187087900690 semiprime?: False
Is 158456325028528675187087900691 semiprime?: False
Is 158456325028528675187087900692 semiprime?: False
Is 158456325028528675187087900693 semiprime?: False
Is 158456325028528675187087900694 semiprime?: False
Is 158456325028528675187087900695 semiprime?: False
Is 158456325028528675187087900696 semiprime?: False
Is 158456325028528675187087900697 semiprime?: False
Is 158456325028528675187087900698 semiprime?: False
Is 158456325028528675187087900699 semiprime?: False
Is 158456325028528675187087900700 semiprime?: False
Is 158456325028528675187087900701 semiprime?: True
elapsed seconds: 0.0574433
```