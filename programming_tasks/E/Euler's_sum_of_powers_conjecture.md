[1]: https://rosettacode.org/wiki/Euler%27s_sum_of_powers_conjecture

# [Euler&#039;s sum of powers conjecture][1]



```perl
constant MAX = 250;

my  %po5{Int};
my %sum2{Int};

for 1..MAX -> \i {
    %po5{i⁵} = i;
    for 1..MAX -> \j {
        %sum2{i⁵ + j⁵} = i, j;
    }
}

%po5.keys.sort.race.map: -> \p {
    for %sum2.keys.sort -> \s {
        if p > s and %sum2{p - s} {
            say ((sort |%sum2{s},|%sum2{p - s}) X~ '⁵').join(' + '), " = %po5{p}", "⁵" and exit
        }
    }
}
```

#### Output:
```
27⁵ + 84⁵ + 110⁵ + 133⁵ = 144⁵
```
