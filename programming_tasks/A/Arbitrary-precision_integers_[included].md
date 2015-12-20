[1]: http://rosettacode.org/wiki/Arbitrary-precision_integers_(included)

# [Arbitrary-precision integers (included)][1]

```perl6
given ~[**] 5, 4, 3, 2 {
   say "5**4**3**2 = {.substr: 0,20}...{.substr: *-20} and has {.chars} digits";
}
```

#### Output:
```
5**4**3**2 = 62060698786608744707...92256259918212890625 and has 183231 digits
```