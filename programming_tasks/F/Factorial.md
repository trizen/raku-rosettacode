[1]: http://rosettacode.org/wiki/Factorial

# [Factorial][1]

`[*]` is a reduction operator that multiplies all the following values together. Note that we don't need to start at 1, since the degenerate case of `[*]()` correctly returns 1, and multiplying by 1 to start off with is silly in any case.

```perl
sub postfix:<!> ( UInt:D $n ) is looser(&prefix:<->) { [*] 2..$n }
say 5!;
```

#### Output:
```
120
```


This approach is much more efficient for repeated use, since it automatically caches.  `[\*]` is the so-called triangular version of [\*].  It returns the intermediate results as a list.  Note that Perl 6 allows you to define constants lazily, which is rather helpful when your constant is of infinite size...

```perl
constant fact = 1, |[\*] 1..*;
say fact[5]
```

#### Output:
```
120
```