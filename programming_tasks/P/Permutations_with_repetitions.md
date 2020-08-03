[1]: https://rosettacode.org/wiki/Permutations_with_repetitions

# [Permutations with repetitions][1]

We can use the `X` operator ("cartesian product") to cross the list with itself.

For ![image](https://rosettacode.org/mw/index.php?title=Special:MathShowImage&hash=c303081f7a16f603112b0375bdc84883&mode=mathml):

```raku
my @k = <a b c>;
 
.say for @k X @k;
```


For arbitrary ![image](https://rosettacode.org/mw/index.php?title=Special:MathShowImage&hash=7b8b965ad4bca0e41ab51de7b31363a1&mode=mathml):

```raku
my @k = <a b c>;
my $n = 2;
 
.say for [X] @k xx $n;
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


Here is an other approach, counting all ![image](https://rosettacode.org/mw/index.php?title=Special:MathShowImage&hash=aafabbf195eb3424ca815b9afe5caff1&mode=mathml) possibilities in base ![image](https://rosettacode.org/mw/index.php?title=Special:MathShowImage&hash=8ce4b16b22b58894aa86c421e8759df3&mode=mathml):

```raku
my @k = <a b c>;
my $n = 2;
 
say @k[.polymod: +@k xx $n-1] for ^@k**$n
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