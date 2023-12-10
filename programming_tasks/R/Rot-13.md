[1]: https://rosettacode.org/wiki/Rot-13

# [Rot-13][1]



```perl
my @abc = 'a'..'z';
my $abc = [@abc».uc, @abc];
put .trans: $abc => $abc».rotate(13) for lines
```

#### Output:
```
Rosetta Code
```

#### Output:
```
Ebfrggn Pbqr
```


As a one-liner:

```console
$ raku -pe '.=trans: {$_ => $_».rotate(13)}({[$_».uc, @$_]}("a".."z"))'
```
