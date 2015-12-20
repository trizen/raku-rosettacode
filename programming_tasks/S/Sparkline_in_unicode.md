[1]: http://rosettacode.org/wiki/Sparkline_in_unicode

# [Sparkline in unicode][1]

```perl6
constant @bars = '▁' ... '█';
while prompt 'Numbers separated by anything: ' -> $_ {
    my @numbers = map +*, .comb(/ '-'? \d+ ['.' \d+]? /);
    my ($mn,$mx) = @numbers.minmax.bounds;
    say "min: $mn.fmt('%5f'); max: $mx.fmt('%5f')";
    my $div = ($mx - $mn) / (@bars - 1);
    say @bars[ (@numbers X- $mn) X/ $div ].join;
}
```

#### Output:
```
Numbers separated by anything: 9 18 27 36 45 54 63 72 63 54 45 36 27 18 9
9 18 27 36 45 54 63 72 63 54 45 36 27 18 9
min: 9.000000; max: 72.000000
▁▂▃▄▅▆▇█▇▆▅▄▃▂▁
Numbers separated by anything: 1.5, 0.5 3.5, 2.5 5.5, 4.5 7.5, 6.5
1.5 0.5 3.5 2.5 5.5 4.5 7.5 6.5
min: 0.500000; max: 7.500000
▂▁▄▃▆▅█▇
Numbers separated by anything: 3 2 1 0 -1 -2 -3 -4 -3 -2 -1 0 1 2 3  
min: -4.000000; max: 3.000000
█▇▆▅▄▃▂▁▂▃▄▅▆▇█
Numbers separated by anything: ^D
```