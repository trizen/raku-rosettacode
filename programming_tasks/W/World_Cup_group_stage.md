[1]: https://rosettacode.org/wiki/World_Cup_group_stage

# [World Cup group stage][1]



```perl
constant scoring = 0, 1, 3;
my @histo = [0 xx 10] xx 4;

for [X] ^3 xx 6 -> @results {
    my @s;

    for @results Z (^4).combinations(2) -> ($r, @g) {
        @s[@g[0]] += scoring[$r];
        @s[@g[1]] += scoring[2 - $r];
    }

    for @histo Z @s.sort -> (@h, $v) {
        ++@h[$v];
    }
}

say .fmt('%3d',' ') for @histo.reverse;
```

#### Output:
```
  0   0   0   1  14 148 152 306   0 108
  0   0   4  33 338 172 164  18   0   0
  0  18 136 273 290   4   8   0   0   0
108 306 184 125   6   0   0   0   0   0
```
