[1]: http://rosettacode.org/wiki/Permutations_with_repetitions

# [Permutations with repetitions][1]

List operators such as X are naturally lazy in Perl 6.

```perl
my $k = <a b c>;
my $n = 2;
 
.say for [X] |($k xx $n);
```

#### Output:
```
a a
a b
a c
b a
b b
b c
c a
c b
c c
```


Here is an other approach, counting all <img class="mwe-math-fallback-image-inline tex" alt="k^n" src="http://rosettacode.org/mw/images/math/e/d/8/ed8ff3c12ebeab3386882a618c78afb8.png"/> possibilities in base <img class="mwe-math-fallback-image-inline tex" alt="k" src="http://rosettacode.org/mw/images/math/8/c/e/8ce4b16b22b58894aa86c421e8759df3.png"/>:

```perl
my @k = <a b c>;
my $n = 2;
say @k[.polymod(+@k xx ($n-1))] for ^@k**$n
```

#### Output:
```
a a
b a
c a
a b
b b
c b
a c
b c
c c
```