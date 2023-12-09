[1]: https://rosettacode.org/wiki/SEND_%2B_MORE_=_MONEY

# [SEND + MORE = MONEY][1]

### Idiomatic

```perl
enum <D E M N O R S Y>;

sub find_solution ( ) {
    for ('0'..'9').combinations(8) -> @c {
        .return with @c.permutations.first: -> @p {
            @p[M] !== 0 and

            @p[  S,E,N,D].join
          + @p[  M,O,R,E].join
         == @p[M,O,N,E,Y].join
        }
    }
}

my @s = find_solution();
say "    {@s[  S,E,N,D].join}";
say " +  {@s[  M,O,R,E].join}";
say "== { @s[M,O,N,E,Y].join}";
```

#### Output:
```
    9567
 +  1085
== 10652
```


### Fast



Alternately, a version written in 2015 by [Carl Mäsak](http://strangelyconsistent.org/blog/send-more-money-in-perl6). Not very concise but quite speedy. Applying the observation that M must be 1 and S must be either 8 or 9 gets the runtime under a tenth of a second.

```perl
my $s = 7;
while ++$s ≤ 9 {
    my $e = -1;
    while ++$e ≤ 9 {
        next if $e == $s;
 
        my $n = -1;
        while ++$n ≤ 9 {
            next if $n == $s|$e;
 
            my $d = -1;
            while ++$d ≤ 9 {
                next if $d == $s|$e|$n;

                my $send = $s×10³ + $e×10² + $n×10 + $d; 
                my ($m, $o) = 1, -1;
                while ++$o ≤ 9 {
                    next if $o == $s|$e|$n|$d|$m;
 
                    my $r = -1;
                    while ++$r ≤ 9 {
                        next if $r == $s|$e|$n|$d|$m|$o;
 
                        my $more = $m×10³ + $o×10² + $r×10 + $e;
                        my $y = -1;
                        while ++$y ≤ 9 {
                            next if $y == $s|$e|$n|$d|$m|$o|$r;

                            my $money = $m×10⁴ + $o×10³ + $n×10² + $e×10 + $y;
                            next unless $send + $more == $money;
                            say 'SEND + MORE == MONEY' ~ "\n$send + $more == $money";
                        }
                    }
                }
            }
        }
    }
}
printf "%.3f elapsed seconds", now - INIT now;
```

#### Output:
```
SEND + MORE == MONEY
9567 + 1085 == 10652
0.080 elapsed seconds
```
