[1]: https://rosettacode.org/wiki/Nice_primes

# [Nice primes][1]

```perl
sub digroot ($r) { .tail given $r, { [+] .comb } ... { .chars == 1 } }
my @is-nice = lazy (0..*).map: { .&is-prime && .&digroot.&is-prime ?? $_ !! False };
say @is-nice[500 ^..^ 1000].grep(*.so).batch(11)».fmt("%4d").join: "\n";
```

#### Output:
```
 509  547  563  569  587  599  601  617  619  641  653
 659  673  677  691  709  727  743  761  797  821  839
 853  857  887  907  911  929  941  947  977  983  997
```


Alternately, with somewhat better separation of concerns.

```perl
sub digroot ($r) { ($r, { .comb.sum } … { .chars == 1 }).tail }
sub is-nice ($_) { .is-prime && .&digroot.is-prime }
say (500 ^..^ 1000).grep( *.&is-nice ).batch(11)».fmt("%4d").join: "\n";
```


Same output.
