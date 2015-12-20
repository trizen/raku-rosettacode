[1]: http://rosettacode.org/wiki/Averages/Simple_moving_average

# [Averages/Simple moving average][1]

```perl
sub sma(Int \P where * > 0) returns Sub {
    sub ($x) {
        state @a = 0 xx P;
        @a.push($x).shift;
        P R/ [+] @a;
    }
}
```