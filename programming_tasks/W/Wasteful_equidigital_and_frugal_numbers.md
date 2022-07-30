[1]: https://rosettacode.org/wiki/Wasteful,_equidigital_and_frugal_numbers

# [Wasteful, equidigital and frugal numbers][1]

```perl
use Prime::Factor;
use Lingua::EN::Numbers;
 
my %cache;
 
sub factor-char-sum ($n, $base = 10) { sum $n.&prime-factors.Bag.map: { .key.base($base).chars + (.value > 1 ?? .value.base($base).chars !! 0) } }
 
sub economical  ($n, $base = 10) { ($n >  1) && $n.base($base).chars >  (%cache{$base}[$n] //= factor-char-sum $n, $base) }
sub equidigital ($n, $base = 10) { ($n == 1) || $n.base($base).chars == (%cache{$base}[$n] //= factor-char-sum $n, $base) }
sub extravagant ($n, $base = 10) {              $n.base($base).chars <  (%cache{$base}[$n] //= factor-char-sum $n, $base) }
 
 
for 10, 11 -> $base {
    %cache{$base}[3e6] = Any; # preallocate to avoid concurrency issues
    say "\nIn Base $base:";
    for &extravagant, &equidigital, &economical -> &sub {
        say "\nFirst 50 {&sub.name} numbers:";
        say (^∞).grep( {.&sub($base)} )[^50].batch(10)».&comma».fmt("%6s").join: "\n";
        say "10,000th: " ~ (^∞).hyper(:2000batch).grep( {.&sub($base)} )[9999].&comma;
    }
 
    my $upto = 1e6.Int;
    my atomicint ($extravagant, $equidigital, $economical);
    say "\nOf the positive integers up to {$upto.&cardinal}:";
    (1..^$upto).race(:5000batch).map: { .&extravagant($base) ?? ++⚛$extravagant !! .&equidigital($base) ?? ++⚛$equidigital !! ++⚛$economical };
    say " Extravagant: {comma $extravagant}\n Equidigital: {comma $equidigital}\n  Economical: {comma $economical}";
    %cache{$base} = Empty;
}
```

#### Output:
```
In Base 10:

First 50 extravagant numbers:
     4      6      8      9     12     18     20     22     24     26
    28     30     33     34     36     38     39     40     42     44
    45     46     48     50     51     52     54     55     56     57
    58     60     62     63     65     66     68     69     70     72
    74     75     76     77     78     80     82     84     85     86
10,000th: 14,346

First 50 equidigital numbers:
     1      2      3      5      7     10     11     13     14     15
    16     17     19     21     23     25     27     29     31     32
    35     37     41     43     47     49     53     59     61     64
    67     71     73     79     81     83     89     97    101    103
   105    106    107    109    111    112    113    115    118    119
10,000th: 33,769

First 50 economical numbers:
   125    128    243    256    343    512    625    729  1,024  1,029
 1,215  1,250  1,280  1,331  1,369  1,458  1,536  1,681  1,701  1,715
 1,792  1,849  1,875  2,048  2,187  2,197  2,209  2,401  2,560  2,809
 3,125  3,481  3,584  3,645  3,721  4,096  4,374  4,375  4,489  4,802
 4,913  5,041  5,103  5,329  6,241  6,250  6,561  6,859  6,889  7,203
10,000th: 1,953,125

Of the positive integers up to one million:
 Extravagant: 831,231
 Equidigital: 165,645
  Economical: 3,123

In Base 11:

First 50 extravagant numbers:
     4      6      8      9     10     12     18     20     22     24
    26     28     30     33     34     36     38     39     40     42
    44     45     46     48     50     51     52     54     55     56
    57     58     60     62     63     65     66     68     69     70
    72     74     75     76     77     78     80     82     84     85
10,000th: 12,890

First 50 equidigital numbers:
     1      2      3      5      7     11     13     14     15     16
    17     19     21     23     25     27     29     31     32     35
    37     41     43     47     49     53     59     61     64     67
    71     73     79     81     83     89     97    101    103    107
   109    113    121    122    123    127    129    131    133    134
10,000th: 33,203

First 50 economical numbers:
   125    128    243    256    343    512    625    729  1,024  1,331
 1,369  1,458  1,536  1,681  1,701  1,715  1,792  1,849  1,875  2,048
 2,187  2,197  2,209  2,401  2,560  2,809  3,072  3,125  3,481  3,584
 3,645  3,721  4,096  4,374  4,375  4,489  4,802  4,913  5,041  5,103
 5,120  5,329  6,241  6,250  6,561  6,859  6,889  7,168  7,203  7,921
10,000th: 2,659,171

Of the positive integers up to one million:
 Extravagant: 795,861
 Equidigital: 200,710
  Economical: 3,428
```
