[1]: https://rosettacode.org/wiki/Penholodigital_squares

# [Penholodigital squares][1]

```perl
(9 .. 12).map: -> $base {
    my $test = (1 ..^ $base)».base($base).join;
    my $start = $test     .parse-base($base).sqrt.Int;
    my $end   = $test.flip.parse-base($base).sqrt.Int;
    say "\nThere is a total of {+$_} penholodigital squares in base $base:\n" ~
        .map({"{.base($base)}² = {.².base($base)}"}).batch(3)».join(", ").join: "\n" given
        ($start .. $end).grep: *².base($base).comb.sort.join eq $test
}

(13 .. 16).hyper(:1batch).map: -> $base {
    my $test = (1 ..^ $base)».base($base).join;
    my $start = $test     .parse-base($base).sqrt.Int;
    my $end   = $test.flip.parse-base($base).sqrt.Int;
    my @penholo = ($start .. $end).grep: *².base($base).comb.sort.join eq $test;
    say "\nThere is a total of {+@penholo} penholodigital squares in base $base:";
    say @penholo[0,*-1].map({"{.base($base)}² = {.².base($base)}"}).batch(3)».join(", ").join: "\n" if +@penholo;
}
```

#### Output:
```
There is a total of 10 penholodigital squares in base 9:
3825² = 16328547, 3847² = 16523874, 4617² = 23875614
4761² = 25487631, 6561² = 47865231, 6574² = 48162537
6844² = 53184267, 7285² = 58624317, 7821² = 68573241
8554² = 82314657

There is a total of 30 penholodigital squares in base 10:
11826² = 139854276, 12363² = 152843769, 12543² = 157326849
14676² = 215384976, 15681² = 245893761, 15963² = 254817369
18072² = 326597184, 19023² = 361874529, 19377² = 375468129
19569² = 382945761, 19629² = 385297641, 20316² = 412739856
22887² = 523814769, 23019² = 529874361, 23178² = 537219684
23439² = 549386721, 24237² = 587432169, 24276² = 589324176
24441² = 597362481, 24807² = 615387249, 25059² = 627953481
25572² = 653927184, 25941² = 672935481, 26409² = 697435281
26733² = 714653289, 27129² = 735982641, 27273² = 743816529
29034² = 842973156, 29106² = 847159236, 30384² = 923187456

There is a total of 20 penholodigital squares in base 11:
42045² = 165742A893, 43152² = 173A652894, 44926² = 18792A6453
47149² = 1A67395824, 47257² = 1A76392485, 52071² = 249A758631
54457² = 2719634A85, 55979² = 286A795314, 59597² = 314672A895
632A4² = 3671A89245, 64069² = 376198A254, 68335² = 41697528A3
71485² = 46928A7153, 81196² = 5A79286413, 83608² = 632A741859
86074² = 6713498A25, 89468² = 7148563A29, 91429² = 76315982A4
93319² = 795186A234, A3A39² = 983251A764

There is a total of 23 penholodigital squares in base 12:
117789² = 135B7482A69, 16357B² = 23A5B976481, 16762B² = 24AB5379861
16906B² = 25386749BA1, 173434² = 26B859A3714, 178278² = 2835BA17694
1A1993² = 34A8125B769, 1A3595² = 354A279B681, 1B0451² = 3824B7569A1
1B7545² = 3A5B2487961, 2084A9² = 42A1583B769, 235273² = 5287BA13469
2528B5² = 5B23A879641, 25B564² = 62937B5A814, 262174² = 63A8527B194
285A44² = 73B615A8294, 29A977² = 7B9284A5361, 2A7617² = 83AB5479261
2B0144² = 8617B35A294, 307381² = 93825A67B41, 310828² = 96528AB7314
319488² = 9AB65823714, 319A37² = 9B2573468A1

There is a total of 0 penholodigital squares in base 13:

There is a total of 160 penholodigital squares in base 14:
1129535² = 126A84D79C53B, 3A03226² = DB3962A7541C8

There is a total of 419 penholodigital squares in base 15:
4240C58² = 12378DA5B6EC94, EE25E4A² = ED4C93285671BA

There is a total of 740 penholodigital squares in base 16:
11156EB6² = 123DA7F85BCE964, 3FD8F786² = FEC81B69573DA24
```
