[1]: http://rosettacode.org/wiki/Temperature_conversion

# [Temperature conversion][1]

```perl6
while my $answer = prompt 'Temperature: ' {
    my $k = do given $answer {
        when s/:i C $// { $_ + 273.15 }
        when s/:i F $// { ($_ + 459.67) / 1.8 }
        when s/:i R $// { $_ / 1.8 }
        when s/:i K $// { $_ }
        default         { $_ }
    }
    say "  { $k }K";
    say "  { $k - 273.15 }℃";
    say "  { $k * 1.8 - 459.67 }℉";
    say "  { $k * 1.8 }R";
}
```

#### Output:
```
Temperature: 0
  0K
  -273.15℃
  -459.67℉
  0R
Temperature: 0c
  273.15K
  0℃
  32℉
  491.67R
Temperature: 212f
  373.15K
  100℃
  212℉
  671.67R
Temperature: -40c
  233.15K
  -40℃
  -40℉
  419.67R
```