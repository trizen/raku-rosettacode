[1]: https://rosettacode.org/wiki/String_concatenation

# [String concatenation][1]



```perl
my $s = 'hello';
say $s ~ ' literal';
my $s1 = $s ~ ' literal';
say $s1;

# or, using mutating concatenation:

$s ~= ' literal';
say $s;
```


Note also that most concatenation in Raku is done implicitly via interpolation.
