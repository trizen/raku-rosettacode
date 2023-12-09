[1]: https://rosettacode.org/wiki/Getting_the_number_of_decimal_places

# [Getting the number of decimal places][1]

Raku does not specifically have a "decimal" number type, however we can
easily determine the fractional precision of a rational number. It is somewhat
touchy-feely for floating point numbers; (what is the fractional precision for
2.45e-12?), it's pretty pointless for Integers; (zero, aalllways zero...), but
Rats (rationals) are doable. Note that these are (mostly) actual numerics, not
numeric strings. The exception is '12.3450'. That is a numeric string since
actual numerics automatically truncate non-significant trailing zeros. If you 
want to retain them, you need to pass the value as a string. (As below.)

```perl
use Rat::Precise;

printf "Fractional precision: %-2s || Number: %s\n", (.split('.')[1] // '').chars, $_
    for 9, 12.345, '12.3450', 0.1234567890987654321, (1.5**63).precise;
```

#### Output:
```
Fractional precision: 0  || Number: 9
Fractional precision: 3  || Number: 12.345
Fractional precision: 4  || Number: 12.3450
Fractional precision: 19 || Number: 0.1234567890987654321
Fractional precision: 63 || Number: 124093581919.648947697827373650380188008224280338254175148904323577880859375
```
