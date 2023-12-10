[1]: https://rosettacode.org/wiki/Coprimes

# [Coprimes][1]

How do you determine if numbers are co-prime? Check to see if the [Greatest common divisor](https://rosettacode.org/wiki/Greatest_common_divisor) is equal to one. Since we're duplicating tasks willy-nilly, lift code from [that task](https://rosettacode.org/wiki/Greatest_common_divisor#Raku), (or in this case, just use the builtin).

```perl
say .raku, ( [gcd] |$_ ) == 1 ?? ' Coprime' !! '' for [21,15],[17,23],[36,12],[18,29],[60,15],[21,22,25,31,143]
```

#### Output:
```
[21, 15]
[17, 23] Coprime
[36, 12]
[18, 29] Coprime
[60, 15]
[21, 22, 25, 31, 143] Coprime
```
