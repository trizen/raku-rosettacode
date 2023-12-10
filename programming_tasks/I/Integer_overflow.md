[1]: https://rosettacode.org/wiki/Integer_overflow

# [Integer overflow][1]





The Raku program below does **not** recognize a signed integer overflow and the program **continues with wrong results**.

```perl
my int64 ($a, $b, $c) = 9223372036854775807, 5000000000000000000, 3037000500;
.say for -(-$a - 1), $b + $b, -$a - $a, $c * $c, (-$a - 1)/-1;
```

#### Output:
```
-9223372036854775808
-8446744073709551616
2
-9223372036709301616
9223372036854775808
```
