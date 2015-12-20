[1]: http://rosettacode.org/wiki/Accumulator_factory

# [Accumulator factory][1]

```perl6
sub accum ($n is copy) { sub { $n += $^x } }
```


Example use:

```perl6
my $a = accum 5;
$a(4.5);
say $a(.5);   # Prints "10".
```


You can also use the "&amp;" sigil to create a function that behaves syntactically like any other function (i.e. no sigil nor parentheses needed to call it):

```perl6
my &b = accum 5;
say b 3;   # Prints "8".
```