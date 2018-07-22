[1]: https://rosettacode.org/wiki/Pythagorean_quadruples

# [Pythagorean quadruples][1]

```perl
my \N = 2200;
my @sq = (0 .. N)»²;
my @not = False xx N;
@not[0] = True;
 
for 1 .. N -> $d {
    my $last = 0;
    for $d ... ($d/3).ceiling -> $a {
        for 1 .. ($a/2).ceiling -> $b {
            last if (my $ab = @sq[$a] + @sq[$b]) > @sq[$d];
            if (@sq[$d] - $ab).sqrt.narrow ~~ Int {
                @not[$d] = True;
                $last = 1;
                last
            }
        }
        last if $last;
    }
}
 
say @not.grep( *.not, :k );
```

#### Output:
```
(1 2 4 5 8 10 16 20 32 40 64 80 128 160 256 320 512 640 1024 1280 2048)
```