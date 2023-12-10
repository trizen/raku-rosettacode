[1]: https://rosettacode.org/wiki/Infinity

# [Infinity][1]


Inf support is required by language spec on all abstract Numeric types (in the absence of subset constraints) including Num, Rat and Int types. Native integers cannot support Inf, so attempting to assign Inf will result in an exception; native floats are expected to follow IEEE standards including +/- Inf and NaN.

```perl
my $x = 1.5/0;       # Failure: catchable error, if evaluated will return: "Attempt to divide by zero ... 
my $y = (1.5/0).Num; # assigns 'Inf'
```
