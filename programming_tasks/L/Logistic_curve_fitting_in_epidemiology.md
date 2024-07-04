[1]: https://rosettacode.org/wiki/Logistic_curve_fitting_in_epidemiology

# [Logistic curve fitting in epidemiology][1]

Original task numbers of cases per day as of April 05 plus updated, as of today, May 11 numbers.

```perl
my $K  = 7_800_000_000; #  population
my $n0 = 27;            #  cases @ day 0

my @Apr05 = <
         27      27      27      44      44      59      59      59      59      59
         59      59      59      60      60      61      61      66      83     219
        239     392     534     631     897    1350    2023    2820    4587    6067
       7823    9826   11946   14554   17372   20615   24522   28273   31491   34933
      37552   40540   43105   45177   60328   64543   67103   69265   71332   73327
      75191   75723   76719   77804   78812   79339   80132   80995   82101   83365
      85203   87024   89068   90664   93077   95316   98172  102133  105824  109695
     114232  118610  125497  133852  143227  151367  167418  180096  194836  213150
     242364  271106  305117  338133  377918  416845  468049  527767  591704  656866
     715353  777796  851308  928436 1000249 1082054 1174652
>;

my @May11 = <
         27      27      27      44      44      59      59      59      59      59
         59      59      59      60      60      61      61      66      83     219
        239     392     534     631     897    1350    2023    2820    4587    6067
       7823    9826   11946   14554   17372   20615   24522   28273   31491   34933
      37552   40540   43105   45177   60328   64543   67103   69265   71332   73327
      75191   75723   76719   77804   78812   79339   80132   80995   82101   83365
      85203   87026   89068   90865   93077   95316   98172  102133  105824  109695
     114235  118613  125497  133853  143259  154774  167418  180094  194843  213149
     242374  271116  305235  338235  377968  416881  468092  527839  591796  656834
     715377  777187  851587  928491 1006063 1087822 1174306 1245601 1316988 1391881
    1476792 1563819 1653160 1734868 1807256 1873639 1953786 2033745 2117297 2199460
    2278484 2350993 2427353 2513399 2579823 2657910 2731217 2832750 2915977 2981542
    3054404 3131487 3216467 3308341 3389459 3468047 3545486 3624789 3714816 3809262
    3899379 3986931 4063525
>;

sub logistic-func ($rate, @y) {
    my $sq = 0;
    for ^@y -> $time {
        my $ert = exp($rate * $time);
        my $δt = ($n0 * $ert) / (1 + $n0 * ($ert-1) / $K) - @y[$time];
        $sq += $δt²;
    }
    $sq
}

sub solve (&f, $guess, \ε, @y) {
    my $fₙ-minus;
    my $fₙ-plus;
    my $rate   = $guess;
    my $fₙ     = f $rate, @y;
    my $Δ      = $rate;
    my $factor = 2;
    while $Δ > ε {
        ($fₙ-minus = f $rate - $Δ, @y) < $fₙ ??
        do {
            $fₙ    = $fₙ-minus;
            $rate -= $Δ;
            $Δ    *= $factor;
        } !!
        ($fₙ-plus = f $rate + $Δ, @y) < $fₙ ??
        do {
            $fₙ    = $fₙ-plus;
            $rate += $Δ;
            $Δ    *= $factor;
        } !!
        $Δ /= $factor
    }
    $rate
}

for @Apr05, 'Dec 31 - Apr 5',
    @May11, 'Dec 31 - May 11' -> @y, $period {
    my $rate = solve(&logistic-func, 0.5, 0, @y);
    my $R0   = exp(12 * $rate);
    say "\nFor period: $period";
    say "Instantaneous rate of growth: r = " , $rate.fmt('%08f');
    say "Reproductive rate: R0 = ", $R0.fmt('%08f');
}
```

#### Output:
```
For period: Dec 31 - Apr 5
Instantaneous rate of growth: r = 0.112302
Reproductive rate: R0 = 3.848279

For period: Dec 31 - May 11
Instantaneous rate of growth: r = 0.093328
Reproductive rate: R0 = 3.064672
```