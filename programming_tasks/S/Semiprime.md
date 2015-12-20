[1]: http://rosettacode.org/wiki/Semiprime

# [Semiprime][1]

Here is a naive, grossly inefficient implementation.

```perl
sub is-semiprime (Int $n --> Bool) {
    not $n.is-prime and
        .is-prime given 
        $n div first $n %% *,
            grep &is-prime, 2 .. *;
}
 
use Test;
my @primes = grep &is-prime, 2 .. 100;
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