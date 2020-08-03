[1]: https://rosettacode.org/wiki/Assertions

# [Assertions][1]

```raku
my $a = (1..100).pick;
$a == 42 or die '$a ain\'t 42';
```


*Note: This example uses an experimental feature, and does not work in the primary Perl 6 compiler, Rakudo.*

```raku
# with a (non-hygienic) macro
macro assert ($x) { "$x or die 'assertion failed: $x'" }
assert('$a == 42');
```