[1]: https://rosettacode.org/wiki/Sisyphus_sequence

# [Sisyphus sequence][1]

```perl
use Math::Primesieve;
use Lingua::EN::Numbers;

my ($exp1, $exp2, $limit1, $limit2)  = 3, 8, 100, 250;
my ($n, $s0, $s1, $p, @S1, %S) =  1, 1, Any, Any, 1;
my $iterator = Math::Primesieve::iterator.new;
my @Nth = ($exp1..$exp2)».exp(10);
my $S2 = BagHash.new;

repeat {
    $n++;
    $s1 = $s0 %% 2 ?? $s0 div 2 !! $s0 + ($p = $iterator.next);
    @S1.push: $s1 if  $n ≤ $limit1;
    $S2.add:  $s1 if $s1 ≤ $limit2;
    %S{$n}{'value', 'prime'} = $s1, $p if $n ∈ @Nth;
    $s0 = $s1;
} until $n == @Nth[*-1];

say "The first $limit1 members of the Sisyphus sequence are:";
say @S1.batch(10)».fmt('%4d').join("\n") ~ "\n";
printf "%12sth member is: %13s with prime: %11s\n", ($_, %S{$_}{'value'}, %S{$_}{'prime'})».&comma for @Nth;
say "\nNumbers under $limit2 that do not occur in the first {comma @Nth[*-1]} terms:";
say (1..$limit2).grep: * ∉ $S2.keys;
say "\nNumbers under $limit2 that occur the most ({$S2.values.max} times) in the first {comma @Nth[*-1]} terms:";
say $S2.keys.grep({ $S2{$_} == $S2.values.max}).sort;
```

#### Output:
```
The first 100 members of the Sisyphus sequence are:
   1    3    6    3    8    4    2    1    8    4
   2    1   12    6    3   16    8    4    2    1
  18    9   28   14    7   30   15   44   22   11
  42   21   58   29   70   35   78   39   86   43
  96   48   24   12    6    3   62   31   92   46
  23   90   45  116   58   29  102   51  130   65
 148   74   37  126   63  160   80   40   20   10
   5  106   53  156   78   39  146   73  182   91
 204  102   51  178   89  220  110   55  192   96
  48   24   12    6    3  142   71  220  110   55

       1,000th member is:           990 with prime:       2,273
      10,000th member is:        24,975 with prime:      30,713
     100,000th member is:       265,781 with prime:     392,111
   1,000,000th member is:     8,820,834 with prime:   4,761,697
  10,000,000th member is:    41,369,713 with prime:  55,900,829
 100,000,000th member is: 1,179,614,168 with prime: 640,692,323

Numbers under 250 that do not occur in the first 100,000,000 terms:
36 72 97 107 115 127 144 167 194 211 214 230 232

Numbers under 250 that occur the most (7 times) in the first 100,000,000 terms:
7 14 28
```
