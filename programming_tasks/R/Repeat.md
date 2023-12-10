[1]: https://rosettacode.org/wiki/Repeat

# [Repeat][1]



```perl
sub repeat (&f, $n) { f() xx $n };

sub example { say rand }

repeat(&example, 3);
```

#### Output:
```
0.435249779778396
0.647701200726486
0.279289335968417
```

#### Output:
```
example() xx 3;
```

#### Output:
```
(say rand) xx 3;
```


Notes on the [`xx`](http://doc.raku.org/language/operators#infix_xx) operator:



General notes:
