[1]: http://rosettacode.org/wiki/Dynamic_variable_names

# [Dynamic variable names][1]

It is not possible to change lexical variable names at run time, but package variables are fair game, include in the GLOBAL package:

```perl
my $vname = prompt 'Variable name: ';
$GLOBAL::($vname) = 42;
say $GLOBAL::($vname);
```