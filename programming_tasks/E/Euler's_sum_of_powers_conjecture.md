[1]: http://rosettacode.org/wiki/Euler's_sum_of_powers_conjecture

# [Euler's sum of powers conjecture][1]

```perl
constant MAX = 250;
 
my %p5{Int};
my %sum2{Int};
 
for 1..MAX -> $i {
    %p5{$i**5} = $i;
    for 1..MAX -> $j {
        %sum2{$i**5 + $j**5} = ($i, $j);
    }
}
 
my @sk = %sum2.keys.sort;
for %p5.keys.sort -> $p {
    for @sk -> $s {
        next if $p <= $s;
        if %sum2{$p - $s} {
            say ((sort |%sum2{$s}[],|%sum2{$p-$s}[]) X~ '⁵').join(' + ') ~ " =  %p5{$p}" ~ "⁵";
            exit;
        }
    }
}
```

#### Output:
```
27⁵ + 84⁵ + 110⁵ + 133⁵ =  144⁵
```