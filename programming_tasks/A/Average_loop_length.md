[1]: https://rosettacode.org/wiki/Average_loop_length

# [Average loop length][1]

```raku
constant MAX_N  = 20;
constant TRIALS = 100;
 
for 1 .. MAX_N -> $N {
    my $empiric = TRIALS R/ [+] find-loop(random-mapping($N)).elems xx TRIALS;
    my $theoric = [+]
        map -> $k { $N ** ($k + 1) R/ [*] flat $k**2, $N - $k + 1 .. $N }, 1 .. $N;
 
    FIRST say " N    empiric      theoric      (error)";
    FIRST say "===  =========  ============  =========";
 
    printf "%3d  %9.4f  %12.4f    (%4.2f%%)\n",
            $N,  $empiric,
                        $theoric, 100 * abs($theoric - $empiric) / $theoric;
}
 
sub random-mapping { hash .list Z=> .roll given ^$^size }
sub find-loop { 0, | %^mapping{*} ...^ { (%){$_}++ } }
```

#### Output:
```
 N    empiric      theoric      (error)
===  =========  ============  =========
  1     1.0000        1.0000    (0.00%)
  2     1.5600        1.5000    (4.00%)
  3     1.7800        1.8889    (5.76%)
  4     2.1800        2.2188    (1.75%)
  5     2.6200        2.5104    (4.37%)
  6     2.8300        2.7747    (1.99%)
  7     3.1200        3.0181    (3.37%)
  8     3.1400        3.2450    (3.24%)
  9     3.4500        3.4583    (0.24%)
 10     3.6700        3.6602    (0.27%)
 11     3.8300        3.8524    (0.58%)
 12     4.3600        4.0361    (8.03%)
 13     3.9000        4.2123    (7.42%)
 14     4.4900        4.3820    (2.46%)
 15     4.9500        4.5458    (8.89%)
 16     4.9800        4.7043    (5.86%)
 17     4.9100        4.8579    (1.07%)
 18     4.9700        5.0071    (0.74%)
 19     5.1000        5.1522    (1.01%)
 20     5.2300        5.2936    (1.20%)
```