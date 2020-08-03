[1]: https://rosettacode.org/wiki/Prime_conspiracy

# [Prime conspiracy][1]

```raku
my @primes := 2, |(3,5,7 ... *).grep: *.is-prime;
 
my %conspiracy;
my $upto = 1_000_000;
 
@primes[^($upto+1)].reduce: -> $a, $b {
    my $d = $b % 10;
    %conspiracy{"$a → $d count:"}++;
    $d;
}
 
say "$_ \tfrequency: {($_.value/$upto*100).round(.01)} %" for %conspiracy.sort;
```

#### Output:
```
1 → 1 count:    42853   frequency: 4.29 %
1 → 3 count:    77475   frequency: 7.75 %
1 → 7 count:    79453   frequency: 7.95 %
1 → 9 count:    50153   frequency: 5.02 %
2 → 3 count:    1       frequency: 0 %
3 → 1 count:    58255   frequency: 5.83 %
3 → 3 count:    39668   frequency: 3.97 %
3 → 5 count:    1       frequency: 0 %
3 → 7 count:    72828   frequency: 7.28 %
3 → 9 count:    79358   frequency: 7.94 %
5 → 7 count:    1       frequency: 0 %
7 → 1 count:    64230   frequency: 6.42 %
7 → 3 count:    68595   frequency: 6.86 %
7 → 7 count:    39603   frequency: 3.96 %
7 → 9 count:    77586   frequency: 7.76 %
9 → 1 count:    84596   frequency: 8.46 %
9 → 3 count:    64371   frequency: 6.44 %
9 → 7 count:    58130   frequency: 5.81 %
9 → 9 count:    42843   frequency: 4.28 %
```