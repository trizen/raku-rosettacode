[1]: https://rosettacode.org/wiki/Evaluate_binomial_coefficients

# [Evaluate binomial coefficients][1]


For a start, you can get the length of the corresponding list of combinations:

```perl
say combinations(5, 3).elems;
```

#### Output:
```
10
```


This method is efficient, as Raku will not actually compute each element of the list, since it actually uses an iterator with a defined `count-only` method.  Such method performs computations in a way similar to the following infix operator:

```perl
sub infix:<choose> { [*] ($^n ... 0) Z/ 1 .. $^p }
say 5 choose 3;
```


A possible optimization would use a symmetry property of the binomial coefficient:

```perl
sub infix:<choose> { [*] ($^n ... 0) Z/ 1 .. min($n - $^p, $p) }
```


One drawback of this method is that it returns a Rat, not an Int.  So we actually may want to enforce the conversion:

```perl
sub infix:<choose> { ([*] ($^n ... 0) Z/ 1 .. min($n - $^p, $p)).Int }
```


And *this* is exactly what the `count-only` method does.
