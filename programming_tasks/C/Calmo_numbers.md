[1]: https://rosettacode.org/wiki/Calmo_numbers

# [Calmo numbers][1]

```perl
use Prime::Factor;
use List::Divvy;

my $upto = 1e3;

my @found = (2..Inf).hyper.grep({
    (so my @d = .&proper-divisors(:s)) &&
    (@d.elems %% 3) &&
    (all @d.batch(3)».sum».is-prime)
}).&upto($upto);

put "{+@found} found before $upto using sums of proper-divisors:\n" ~
@found.batch(10)».fmt("%4d").join: "\n";

@found = (2..Inf).hyper.grep({
    (so my @d = .&proper-divisors(:s).&after: 1) &&
    (@d.elems %% 3) &&
    (all @d.batch(3)».sum».is-prime)
}).&upto($upto);

put "\n{+@found} found before $upto using sums of some bizarre\nbespoke definition for divisors:\n" ~
@found.batch(10)».fmt("%4d").join: "\n";
```

#### Output:
```
85 found before 1000 using sums of proper-divisors:
   8   21   27   35   39   55   57   65   77   85
 111  115  125  129  155  161  185  187  201  203
 205  209  221  235  237  265  291  299  305  309
 319  323  327  335  341  365  371  377  381  391
 413  415  437  451  485  489  493  497  505  515
 517  535  579  611  623  649  655  667  669  671
 687  689  697  707  731  737  755  767  779  781
 785  831  835  851  865  893  899  901  917  921
 939  955  965  979  989

9 found before 1000 using sums of some bizarre
bespoke definition for divisors:
 165  273  385  399  561  595  665  715  957
```
