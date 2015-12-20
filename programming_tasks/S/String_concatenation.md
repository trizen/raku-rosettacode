[1]: http://rosettacode.org/wiki/String_concatenation

# [String concatenation][1]

```perl6
my $s = 'hello';
say $s ~ ' literal';
my $s1 = $s ~ ' literal';
say $s1;
```


An example of mutating concatenation:

```perl6
$s ~= ' literal';
say $s;
```


Note also that most concatenation in Perl is done implicitly via interpolation.