[1]: https://rosettacode.org/wiki/Sub-unit_squares

# [Sub-unit squares][1]

First seven take about 5 seconds with this implementation. The eighth would take several hours at least.

```perl
my @upper = 1,|(1 .. *).map((* × 2)²);
my @lower = (^∞).map: *²;
my \R = [\+] 0, 1, 10, 100 … *;

my $l = 0;

.say for (gather {
    (^∞).map: -> $u {
        next if @upper[$u].contains: 0;
        my $chars = @upper[$u].chars;
        loop {
            $l++ and next   if @upper[$u] - @lower[$l]  > R[$chars];
            take @upper[$u] if @upper[$u] - @lower[$l] == R[$chars];
            last;
        }
    }
})[^7]
```

#### Output:
```
1
36
3136
24336
5973136
71526293136
318723477136
```
