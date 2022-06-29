[1]: https://rosettacode.org/wiki/Prime_triangle

# [Prime triangle][1]

Limit the upper threshold a bit to avoid multiple hours of pointless calculations. Even just up to 17 takes over 20 minutes.

```perl
my @count = 0, 0, 1;
my $lock = Lock.new;
put (1,2);
 
for 3..17 -> $n {
    my @even = (2..^$n).grep: * %% 2;
    my @odd  = (3..^$n).grep: so * % 2;
    @even.permutations.race.map: -> @e {
        quietly next if @e[0] == 8|14;
        my $nope = 0;
        for @odd.permutations -> @o {
            quietly next unless (@e[0] + @o[0]).is-prime;
            my @list;
            for (@list = (flat (roundrobin(@e, @o)), $n)).rotor(2 => -1) {
                $nope++ and last unless .sum.is-prime;
            }
            unless $nope {
                put '1 ', @list unless @count[$n];
                $lock.protect({ @count[$n]++ });
            }
            $nope = 0;
        }
    }
}
put "\n", @count[2..*];
```

#### Output:
```
1 2
1 2 3
1 2 3 4
1 4 3 2 5
1 4 3 2 5 6
1 4 3 2 5 6 7
1 2 3 4 7 6 5 8
1 2 3 4 7 6 5 8 9
1 2 3 4 7 6 5 8 9 10
1 6 5 8 3 10 7 4 9 2 11
1 6 5 8 3 10 7 4 9 2 11 12
1 4 3 2 5 8 9 10 7 12 11 6 13
1 4 3 2 11 8 9 10 13 6 7 12 5 14
1 2 3 8 5 12 11 6 7 10 13 4 9 14 15
1 2 3 8 5 12 11 6 7 10 13 4 9 14 15 16
1 2 9 4 7 10 13 6 5 14 3 16 15 8 11 12 17

1 1 1 1 1 2 4 7 24 80 216 648 1304 3392 13808 59448
```
