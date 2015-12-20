[1]: http://rosettacode.org/wiki/Price_fraction

# [Price fraction][1]

```perl6
my $table = q:to/END/;
>=  0.00  <  0.06  :=  0.10
>=  0.06  <  0.11  :=  0.18
>=  0.11  <  0.16  :=  0.26
>=  0.16  <  0.21  :=  0.32
>=  0.21  <  0.26  :=  0.38
>=  0.26  <  0.31  :=  0.44
>=  0.31  <  0.36  :=  0.50
>=  0.36  <  0.41  :=  0.54
>=  0.41  <  0.46  :=  0.58
>=  0.46  <  0.51  :=  0.62
>=  0.51  <  0.56  :=  0.66
>=  0.56  <  0.61  :=  0.70
>=  0.61  <  0.66  :=  0.74
>=  0.66  <  0.71  :=  0.78
>=  0.71  <  0.76  :=  0.82
>=  0.76  <  0.81  :=  0.86
>=  0.81  <  0.86  :=  0.90
>=  0.86  <  0.91  :=  0.94
>=  0.91  <  0.96  :=  0.98
>=  0.96  <  1.01  :=  1.00
END
 
my $value = 0.44;
 
say price($value);
 
sub price($value)
{
	for $table.lines -> $line {
		$line ~~ / '>=' \s+ (\S+) \s+ '<' \s+ (\S+) \s+ ':=' \s+ (\S+)/;
		return $2 if $0 <= $value < $1;
	}
	fail "Out of range";
}
```


Perhaps a better approach is just to build an array of 101 entries.
Memory is cheap, and array lookup is blazing fast, especially important if used in a loop as below.
Moreover, in Perl&#160;6 we don't have to worry about floating point misrepresentations of decimals because decimal fractions are stored as rationals.

```perl6
my @price = map *.value, flat
    ( 0 ..^ 6  X=> 0.10),
    ( 6 ..^ 11 X=> 0.18),
    (11 ..^ 16 X=> 0.26),
    (16 ..^ 21 X=> 0.32),
    (21 ..^ 26 X=> 0.38),
    (26 ..^ 31 X=> 0.44),
    (31 ..^ 36 X=> 0.50),
    (36 ..^ 41 X=> 0.54),
    (41 ..^ 46 X=> 0.58),
    (46 ..^ 51 X=> 0.62),
    (51 ..^ 56 X=> 0.66),
    (56 ..^ 61 X=> 0.70),
    (61 ..^ 66 X=> 0.74),
    (66 ..^ 71 X=> 0.78),
    (71 ..^ 76 X=> 0.82),
    (76 ..^ 81 X=> 0.86),
    (81 ..^ 86 X=> 0.90),
    (86 ..^ 91 X=> 0.94),
    (91 ..^ 96 X=> 0.98),
    (96 ..^101 X=> 1.00),
;
 
while prompt("value: ") -> $value {
    say @price[ $value * 100 ] // note "Out of range";
}
```


Yet another approach is to use the conditional operator to encode the table.
This allows each endpoint to be written once, avoiding duplication. The <tt>Rat()</tt>
type in the signature coerces any numeric type to a rational.

```perl6
sub price_fraction ( Rat() $n where { $^n >= 0 and $^n <= 1 } ) {
       ( $n <  0.06 ) ?? 0.10
    !! ( $n <  0.11 ) ?? 0.18
    !! ( $n <  0.16 ) ?? 0.26
    !! ( $n <  0.21 ) ?? 0.32
    !! ( $n <  0.26 ) ?? 0.38
    !! ( $n <  0.31 ) ?? 0.44
    !! ( $n <  0.36 ) ?? 0.50
    !! ( $n <  0.41 ) ?? 0.54
    !! ( $n <  0.46 ) ?? 0.58
    !! ( $n <  0.51 ) ?? 0.62
    !! ( $n <  0.56 ) ?? 0.66
    !! ( $n <  0.61 ) ?? 0.70
    !! ( $n <  0.66 ) ?? 0.74
    !! ( $n <  0.71 ) ?? 0.78
    !! ( $n <  0.76 ) ?? 0.82
    !! ( $n <  0.81 ) ?? 0.86
    !! ( $n <  0.86 ) ?? 0.90
    !! ( $n <  0.91 ) ?? 0.94
    !! ( $n <  0.96 ) ?? 0.98
    !!                   1.00
    ;
}
 
while prompt("value: ") -> $value {
    last if $value ~~ /exit|quit/;
    say price_fraction(+$value);
}
```