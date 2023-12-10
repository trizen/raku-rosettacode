[1]: https://rosettacode.org/wiki/Paraffins

# [Paraffins][1]





Counting only, same algorithm as the C solution with some refactorings.



Note how lexical scoping &#8212; rather than global variables or repeated arguments &#8212; is used to pass down information to subroutines.

```perl
sub count-unrooted-trees(Int $max-branches, Int $max-weight) {
    my @rooted   = flat 1,1,0 xx $max-weight - 1;
    my @unrooted = flat 1,1,0 xx $max-weight - 1;

    sub count-trees-with-centroid(Int $radius) {
        sub add-branches(
            Int $branches,        # number of branches to add
            Int $w,               # weight of heaviest branch to add
            Int $weight  is copy, # accumulated weight of tree
            Int $choices is copy, # number of choices so far
        ) {
            $choices *= @rooted[$w];
            for 1 .. $branches -> $b {
                ($weight += $w) <= $max-weight or last;
                @unrooted[$weight] += $choices if $weight > 2*$radius;
                if $b < $branches {
                    @rooted[$weight] += $choices;
                    add-branches($branches - $b, $_, $weight, $choices) for 1 ..^ $w;
                    $choices = $choices * (@rooted[$w] + $b) div ($b + 1);
                }
            }
        }
        add-branches($max-branches, $radius, 1, 1);
    }

    sub count-trees-with-bicentroid(Int $weight) {
        if $weight %% 2 {
            my \halfs = @rooted[$weight div 2];
            @unrooted[$weight] += (halfs * (halfs + 1)) div 2;
        }
    }

    gather {
        take 1;
        for 1 .. $max-weight {
            count-trees-with-centroid($_);
            count-trees-with-bicentroid($_);
            take @unrooted[$_];
        }
    }
}

my constant N = 100;
my @paraffins = count-unrooted-trees(4, N);
say .fmt('%3d'), ': ', @paraffins[$_] for flat 1 .. 30, N;
```

#### Output:
```
  1: 1
  2: 1
  3: 1
  4: 2
  5: 3
  6: 5
  7: 9
  8: 18
  9: 35
 10: 75
 11: 159
 12: 355
 13: 802
 14: 1858
 15: 4347
 16: 10359
 17: 24894
 18: 60523
 19: 148284
 20: 366319
 21: 910726
 22: 2278658
 23: 5731580
 24: 14490245
 25: 36797588
 26: 93839412
 27: 240215803
 28: 617105614
 29: 1590507121
 30: 4111846763
100: 5921072038125809849884993369103538010139
```
