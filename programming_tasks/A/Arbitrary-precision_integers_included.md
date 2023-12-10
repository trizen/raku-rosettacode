[1]: https://rosettacode.org/wiki/Arbitrary-precision_integers_(included)

# [Arbitrary-precision integers (included)][1]



```perl
given [**] 5, 4, 3, 2 {
  use Test;
  ok /^ 62060698786608744707 <digit>* 92256259918212890625 $/,
     '5**4**3**2 has expected first and last twenty digits';
  printf 'This number has %d digits', .chars;
}
```

#### Output:
```
ok 1 - 5**4**3**2 has expected first and last twenty digits
This number has 183231 digits
```
