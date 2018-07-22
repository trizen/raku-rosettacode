[1]: https://rosettacode.org/wiki/Price_fraction

# [Price fraction][1]

Simple solution, doing a linear search.

Note that in Perl&#160;6 we don't have to worry about floating-point misrepresentations of decimals, because decimal fractions are stored as rationals.

```perl
sub price-fraction ($n where 0..1) {
    when $n < 0.06 { 0.10 }
    when $n < 0.11 { 0.18 }
    when $n < 0.16 { 0.26 }
    when $n < 0.21 { 0.32 }
    when $n < 0.26 { 0.38 }
    when $n < 0.31 { 0.44 }
    when $n < 0.36 { 0.50 }
    when $n < 0.41 { 0.54 }
    when $n < 0.46 { 0.58 }
    when $n < 0.51 { 0.62 }
    when $n < 0.56 { 0.66 }
    when $n < 0.61 { 0.70 }
    when $n < 0.66 { 0.74 }
    when $n < 0.71 { 0.78 }
    when $n < 0.76 { 0.82 }
    when $n < 0.81 { 0.86 }
    when $n < 0.86 { 0.90 }
    when $n < 0.91 { 0.94 }
    when $n < 0.96 { 0.98 }
    default        { 1.00 }
}
 
while prompt("value: ") -> $value {
    say price-fraction(+$value);
}
```


If we expect to rescale many prices, a better approach would be to build a look-up array of 101 entries.
Memory is cheap, and array indexing is blazing fast.

```perl
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
    say @price[$value * 100] // "Out of range";
}
```


We can also build this same look-up array by parsing the table as formatted in the task description:

```perl
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
 
my @price;
 
for $table.lines {
    /:s '>='  (\S+)  '<'  (\S+)  ':='  (\S+)/;
    @price[$0*100 ..^ $1*100] »=» +$2;
}
 
while prompt("value: ") -> $value {
    say @price[$value * 100] // "Out of range";
}
```