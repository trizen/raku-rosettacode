[1]: https://rosettacode.org/wiki/Exponential_digital_sums

# [Exponential digital sums][1]

Implement a lazy generator. Made some assumptions about search limits. May be poor assumptions, but haven't been able to find any counterexamples. (Edit: and some were bad.)

```perl
my @expsum = lazy (2..*).hyper.map( -> $Int {
    my atomicint $miss = 0;
    (2..$Int).map( -> $exp {
        if (my $sum = ($Int ** $exp).comb.sum) > $Int { last if ++⚛$miss > 20 }
        $sum == $Int ?? "$Int^$exp" !! Empty;
    }) || Empty;
});

say "First twenty-five integers that are equal to the digital sum of that integer raised to some power:";
put .join(', ') for @expsum[^25];
say "\nFirst thirty that satisfy that condition in three or more ways:";
put .join(', ') for @expsum.grep({.elems≥3})[^30];
```

#### Output:
```
First twenty-five integers that are equal to the digital sum of that integer raised to some power:
7^4
8^3
9^2
17^3
18^3, 18^6, 18^7
20^13
22^4
25^4
26^3
27^3, 27^7
28^4, 28^5
31^7
34^7
35^5
36^4, 36^5
40^13
43^7
45^6
46^5, 46^8
53^7
54^6, 54^8, 54^9
58^7
63^8
64^6
68^7

First thirty that satisfy that condition in three or more ways:
18^3, 18^6, 18^7
54^6, 54^8, 54^9
90^19, 90^20, 90^21, 90^22, 90^28
107^11, 107^13, 107^15
181^16, 181^18, 181^19, 181^20
360^45, 360^46, 360^49, 360^51
370^48, 370^54, 370^57, 370^59
388^32, 388^35, 388^36
523^39, 523^42, 523^44, 523^45
603^44, 603^47, 603^54
667^48, 667^54, 667^58
793^57, 793^60, 793^64
1062^72, 1062^77, 1062^81
1134^78, 1134^80, 1134^82, 1134^86
1359^92, 1359^98, 1359^102
1827^121, 1827^126, 1827^131
1828^123, 1828^127, 1828^132
2116^140, 2116^143, 2116^147
2330^213, 2330^215, 2330^229
2430^217, 2430^222, 2430^223, 2430^229, 2430^230
2557^161, 2557^166, 2557^174
2610^228, 2610^244, 2610^246
2656^170, 2656^172, 2656^176
2700^406, 2700^414, 2700^420, 2700^427
2871^177, 2871^189, 2871^190
2934^191, 2934^193, 2934^195
3077^187, 3077^193, 3077^199
3222^189, 3222^202, 3222^210
3231^203, 3231^207, 3231^209
3448^215, 3448^221, 3448^227
```
