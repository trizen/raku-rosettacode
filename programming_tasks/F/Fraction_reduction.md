[1]: https://rosettacode.org/wiki/Fraction_reduction

# [Fraction reduction][1]

```raku
my %reduced;
my $digits = 2..4;
 
for $digits.map: * - 1 -> $exp {
    my $start = sum (0..$exp).map( { 10 ** $_ * ($exp - $_ + 1) });
    my $end   = 10**($exp+1) - sum (^$exp).map( { 10 ** $_ * ($exp - $_) } ) - 1;
 
    ($start ..^ $end).race(:8degree, :3batch).map: -> $den {
        next if $den.contains: '0';
        next if $den.comb.unique <= $exp;
 
        for $start ..^ $den -> $num {
            next if $num.contains: '0';
            next if $num.comb.unique <= $exp;
 
            my $set = ($den.comb.head(* - 1).Set ∩ $num.comb.skip(1).Set);
            next if $set.elems < 1;
 
            for $set.keys {
                my $ne = $num.trans: $_ => '', :delete;
                my $de = $den.trans: $_ => '', :delete;
                if $ne / $de == $num / $den {
                    print "\b" x 40, "$num/$den:$_ => $ne/$de";
                    %reduced{"$num/$den:$_"} = "$ne/$de";
                }
            }
        }
    }
 
 
    print "\b" x 40, ' ' x 40, "\b" x 40;
 
    my $digit = $exp +1;
    my %d = %reduced.pairs.grep: { .key.chars == ($digit * 2 + 3) };
    say "\n({+%d}) $digit digit reduceable fractions:";
    for 1..9 {
        my $cnt = +%d.pairs.grep( *.key.contains: ":$_" );
        next unless $cnt;
        say "  $cnt with removed $_";
    }
    say "\n  12 Random (or all, if less) $digit digit reduceable fractions:";
    say "    {.key.substr(0, $digit * 2 + 1)} => {.value} removed {.key.substr(* - 1)}"
      for %d.pairs.pick(12).sort;
}
```

#### Output:
```
(4) 2 digit reduceable fractions:
  2 with removed 6
  2 with removed 9

  12 Random (or all, if less) 2 digit reduceable fractions:
    16/64 => 1/4 removed 6
    19/95 => 1/5 removed 9
    26/65 => 2/5 removed 6
    49/98 => 4/8 removed 9

(122) 3 digit reduceable fractions:
  9 with removed 3
  1 with removed 4
  6 with removed 5
  15 with removed 6
  16 with removed 7
  15 with removed 8
  60 with removed 9

  12 Random (or all, if less) 3 digit reduceable fractions:
    149/298 => 14/28 removed 9
    154/352 => 14/32 removed 5
    165/264 => 15/24 removed 6
    176/275 => 16/25 removed 7
    187/286 => 17/26 removed 8
    194/291 => 14/21 removed 9
    286/385 => 26/35 removed 8
    286/682 => 26/62 removed 8
    374/572 => 34/52 removed 7
    473/572 => 43/52 removed 7
    492/984 => 42/84 removed 9
    594/693 => 54/63 removed 9

(660) 4 digit reduceable fractions:
  14 with removed 1
  25 with removed 2
  92 with removed 3
  14 with removed 4
  29 with removed 5
  63 with removed 6
  16 with removed 7
  17 with removed 8
  390 with removed 9

  12 Random (or all, if less) 4 digit reduceable fractions:
    1348/4381 => 148/481 removed 3
    1598/3196 => 158/316 removed 9
    1783/7132 => 178/712 removed 3
    1978/5934 => 178/534 removed 9
    2971/5942 => 271/542 removed 9
    2974/5948 => 274/548 removed 9
    3584/4592 => 384/492 removed 5
    3791/5798 => 391/598 removed 7
    3968/7936 => 368/736 removed 9
    4329/9324 => 429/924 removed 3
    4936/9872 => 436/872 removed 9
    6327/8325 => 627/825 removed 3
```
