[1]: https://rosettacode.org/wiki/Prime_reciprocal_sum

# [Prime reciprocal sum][1]

The sixteenth is very slow to emerge. Didn't bother trying for the seventeenth. OEIS only lists the first fourteen values.

```perl
sub abbr ($_) { .chars < 41 ?? $_ !! .substr(0,20) ~ '..' ~ .substr(*-20) ~ " ({.chars} digits)" }

sub next-prime {
    state $sum = 0;
    my $next = ((1 / (1 - $sum)).ceiling+1..*).hyper(:2batch).grep(&is-prime)[0];
    $sum += FatRat.new(1,$next);
    $next;
}

printf "%2d: %s\n", $_, abbr next-prime for 1..15;
```

#### Output:
```
 1: 2
 2: 3
 3: 7
 4: 43
 5: 1811
 6: 654149
 7: 27082315109
 8: 153694141992520880899
 9: 337110658273917297268061074384231117039
10: 84241975970641143191..13803869133407474043 (76 digits)
11: 20300753813848234767..91313959045797597991 (150 digits)
12: 20323705381471272842..21649394434192763213 (297 digits)
13: 12748246592672078196..20708715953110886963 (592 digits)
14: 46749025165138838243..65355869250350888941 (1180 digits)
15: 11390125639471674628..31060548964273180103 (2358 digits)
16: 36961763505630520555..02467094377885929191 (4711 digits)
```
