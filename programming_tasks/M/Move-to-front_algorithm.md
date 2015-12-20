[1]: http://rosettacode.org/wiki/Move-to-front_algorithm

# [Move-to-front algorithm][1]

```perl6
sub encode ( Str $word ) {
    my @sym = 'a' .. 'z';
    gather for $word.comb -> $c {
	die "Symbol '$c' not found in @sym" if $c eq @sym.none;
        @sym[0 .. take (@sym ... $c).end] .= rotate(-1);
    }
}
 
sub decode ( @enc ) {
    my @sym = 'a' .. 'z';
    [~] gather for @enc -> $pos {
        take @sym[$pos];
        @sym[0..$pos] .= rotate(-1);
    }
}
 
use Test;
plan 3;
for <broood bananaaa hiphophiphop> -> $word {
    my $enc = encode($word);
    my $dec = decode($enc);
    is $word, $dec, "$word.fmt('%-12s') ($enc[])";
}
```

#### Output:
```
1..3
ok 1 - broood       (1 17 15 0 0 5)
ok 2 - bananaaa     (1 1 13 1 1 1 0 0)
ok 3 - hiphophiphop (7 8 15 2 15 2 2 3 2 2 3 2)
```