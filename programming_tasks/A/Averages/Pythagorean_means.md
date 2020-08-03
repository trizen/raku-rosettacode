[1]: https://rosettacode.org/wiki/Averages/Pythagorean_means

# [Averages/Pythagorean means][1]

```raku
sub A { ([+] @_) / @_ }
sub G { ([*] @_) ** (1 / @_) }
sub H { @_ / [+] 1 X/ @_ }
 
say "A(1,...,10) = ", A(1..10);
say "G(1,...,10) = ", G(1..10);
say "H(1,...,10) = ", H(1..10);
 
```

#### Output:
```
A(1,...,10) = 5.5
G(1,...,10) = 4.52872868811677
H(1,...,10) = 3.41417152147406
```