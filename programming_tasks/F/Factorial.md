[1]: https://rosettacode.org/wiki/Factorial

# [Factorial][1]





### via User-defined Postfix Operator



`[*]` is a reduction operator that multiplies all the following values together. Note that we don't need to start at 1, since the degenerate case of `[*]()` correctly returns 1, and multiplying by 1 to start off with is silly in any case.

```perl
sub postfix:<!> (Int $n) { [*] 2..$n }
say 5!;
```

#### Output:
```
120
```


### via Memoized Constant Sequence



This approach is much more efficient for repeated use, since it automatically caches.  `[\*]` is the so-called triangular version of [\*].  It returns the intermediate results as a list.  Note that Raku allows you to define constants lazily, which is rather helpful when your constant is of infinite size...

```perl
constant fact = 1, |[\*] 1..*;
say fact[5]
```

#### Output:
```
120
```
