[1]: http://rosettacode.org/wiki/Evaluate_binomial_coefficients

# [Evaluate binomial coefficients][1]

For a start, you can get the length of the corresponding list of combinations:

```perl6
say combinations(5, 3).elems;
say 5 choose 3;
```

#### Output:
```
10
```


This method is efficient, as Perl 6 will not actually compute each element of the list, since it actually uses an iterator with a defined <tt>count-only</tt> method. Such method performs computations in a way similar to the following infix operator:

```perl6
sub infix:<choose> { [*] ($^n ... 0) Z/ 1 .. $^p }
```


A possible optimization would use a symmetry property of the binomial coefficient:

```perl6
sub infix:<choose> { [*] ($^n ... 0) Z/ 1 .. min($n - $^p, $p) }
```


One drawback of this method is that it returns a Rat, not an Int. So we actually may want to enforce the conversion:

```perl6
sub infix:<choose> { ([*] ($^n ... 0) Z/ 1 .. min($n - $^p, $p)).Int }
```


And _this_ is exactly what the <tt>count-only</tt> method does.