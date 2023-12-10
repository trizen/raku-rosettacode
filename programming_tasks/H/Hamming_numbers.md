[1]: https://rosettacode.org/wiki/Hamming_numbers

# [Hamming numbers][1]





### Merge version



The limit scaling is not <em>required</em>, but it cuts down on a bunch of unnecessary calculation.

```perl
my $limit = 32;

sub powers_of ($radix) { 1, |[\*] $radix xx * }

my @hammings = 
  (   powers_of(2)[^ $limit ]       X*
      powers_of(3)[^($limit * 2/3)] X* 
      powers_of(5)[^($limit * 1/2)]
   ).sort;

say @hammings[^20];
say @hammings[1690]; # zero indexed
```

#### Output:
```
(1 2 3 4 5 6 8 9 10 12 15 16 18 20 24 25 27 30 32 36)
2125764000
```


### Iterative version



This version uses a lazy list, storing a maximum of two extra value from the highest index requested

```perl
my \Hammings := gather {
  my %i = 2, 3, 5 Z=> (Hammings.iterator for ^3);
  my %n = 2, 3, 5 Z=> 1 xx 3;

  loop {
    take my $n := %n{2, 3, 5}.min;

    for 2, 3, 5 -> \k {
      %n{k} = %i{k}.pull-one * k if %n{k} == $n;
    }
  }
}
  
say Hammings.[^20];
say Hammings.[1691 - 1];
say Hammings.[1000000 - 1];
```

#### Output:
```
(1 2 3 4 5 6 8 9 10 12 15 16 18 20 24 25 27 30 32 36)
2125764000
519312780448388736089589843750000000000000000000000000000000000000000000000000000000
```
