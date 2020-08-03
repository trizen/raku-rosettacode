[1]: https://rosettacode.org/wiki/Möbius_function

# [Möbius function][1]

Möbius number is not defined for n == 0. Perl 6 arrays are indexed from 0 so store a blank value at position zero to keep n and μ(n) aligned.

```perl
use Prime::Factor;
 
sub μ (Int \n) {
    return 0 if n %% 4 or n %% 9 or n %% 25 or n %% 49 or n %% 121;
    my @p = prime-factors(n);
    +@p == +@p.unique ?? +@p %% 2 ?? 1 !! -1 !! 0
}
 
my @möbius = lazy flat '', 1, (2..*).hyper.map: -> \n { μ(n) };
 
# The Task
put "Möbius sequence - First 199 terms:\n",
    @möbius[^200]».fmt('%3s').batch(20).join: "\n";
```

#### Output:
```
Möbius sequence - First 199 terms:
      1  -1  -1   0  -1   1  -1   0   0   1  -1   0  -1   1   1   0  -1   0  -1
  0   1   1  -1   0   0   1   0   0  -1  -1  -1   0   1   1   1   0  -1   1   1
  0  -1  -1  -1   0   0   1  -1   0   0   0   1   0  -1   0   1   0   1   1  -1
  0  -1   1   0   0   1  -1  -1   0   1  -1  -1   0  -1   1   0   0   1  -1  -1
  0   0   1  -1   0   1   1   1   0  -1   0   1   0   1   1   1   0  -1   0   0
  0  -1  -1  -1   0  -1   1  -1   0  -1  -1   1   0  -1  -1   1   0   0   1   1
  0   0   1   1   0   0   0  -1   0   1  -1  -1   0   1   1   0   0  -1  -1  -1
  0   1   1   1   0   1   1   0   0  -1   0  -1   0   0  -1   1   0  -1   1   1
  0   1   0  -1   0  -1   1  -1   0   0  -1   0   0  -1  -1   0   0   1   1  -1
  0  -1  -1   1   0   1  -1   1   0   0  -1  -1   0  -1   1  -1   0  -1   0  -1
```
