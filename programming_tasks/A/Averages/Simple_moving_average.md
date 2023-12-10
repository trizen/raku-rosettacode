[1]: https://rosettacode.org/wiki/Averages/Simple_moving_average

# [Averages/Simple moving average][1]



```perl
sub sma-generator (Int $P where * > 0) {
    sub ($x) {
        state @a = 0 xx $P;
        @a.push($x).shift;
        @a.sum / $P;
    }
}

# Usage:
my &sma = sma-generator 3;

for 1, 2, 3, 2, 7 {
    printf "append $_ --> sma = %.2f  (with period 3)\n", sma $_;
}
```

#### Output:
```
append 1 --> sma = 0.33  (with period 3)
append 2 --> sma = 1.00  (with period 3)
append 3 --> sma = 2.00  (with period 3)
append 2 --> sma = 2.33  (with period 3)
append 7 --> sma = 4.00  (with period 3)
```
