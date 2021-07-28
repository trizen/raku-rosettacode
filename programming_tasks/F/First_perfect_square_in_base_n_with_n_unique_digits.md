[1]: https://rosettacode.org/wiki/First_perfect_square_in_base_n_with_n_unique_digits

# [First perfect square in base n with n unique digits][1]





As long as you have the patience, this will work for bases 2 through 36.



Bases 2 through 19 finish quickly, (about 10 seconds on my system), 20 takes a while, 21 is pretty fast, 22 is glacial. 23 through 26 takes several hours.



Use analytical start value filtering based on observations by [Hout](https://rosettacode.org/mw/index.php?title=User:Hout&amp;action=edit&amp;redlink=1)++
and [Nigel Galloway](https://rosettacode.org/mw/index.php?title=User:Nigel_Galloway&amp;action=edit&amp;redlink=1)++ on the
[discussion page](https://rosettacode.org/wiki/Talk:First_perfect_square_in_base_N_with_N_unique_digits#Space_compression_and_proof_.3F).



[Try it online!](https://tio.run/##dVTNbtpAEL7zFBPXBDuBlQ1SqkDz0x4i5VBeoGqQA@uwlVmb3XVThMi5fZ1ce@NR@iJ0Zm1jO1UtYS8zO9/8fDOTcZVcHA65FAZ0/gifP95PwXONWHEFV3AXJZr7k06HdLFQ2gz0Oo8UB@9eGnClD9sO4LPawK02kTJoFCeRgV7Y60MvwJc3BMYe6O7@N3uMNB/jGSHJTMR4hmsIA9jCO4hklGyMmGurLHFdlaYGEHchnoSJkgH990p37FsqZB/GhOtROBhsw/aW7mq09YaMoZqtosw727/awxh9tjDdGatwWpCwq0EpYhvQn5@/KvjtUVuFnMax5lSLlZBecYs9KY4uzzDbAsCHQXGatMyLvL6E5yXIV0Sp8F7@1dbGu07xrvhwKz4alWJZpDQfVKkxvUbFnItEyKfJkUhbFKqa4t853gfv4S17Rx/r@izTZ6IJP7WMGgllQcNCFec4VVWIjGFZ6iIiKJqw/WuLSpcCQtWRorY2rv@6Md4MsZkWkETaQC4TrjXasnkqTSSkxhxmNoAy1Ta9RfOfnpateWmPM@h2IZwFQUC/N5QXaZ7b3InVYwmqR0cbcKaoGANs3dlu/1okAx9cfQ3YiF5t6TOV5nLhsSAI/Z124KUFRY9DJtZp6y7wJMo0Xzht55aYBi91s9Aj@Q9j025WUNPwu7NaQoXs1IY6U0Ka2APnE7IB3eFiDN1wpCmvK@gORoF2@ui4T1CNoWryhwNQZ1atnJub8gjn8J@SwMkJ9HpI/65YS80RRmP7ERrmabbBKXbJHfVDUK2q4sZV8cWWWD3arbAdjy5wA/g7nIp8VUZJbx@elyLh1f0lThC1xaQGa02VNSmCQ9LBuaOtCRlXMZ/jji2257MwS5hiZ4p1zqGcNyHBBjsdAzJI1uy0tXOpY2m5kmdaqhAO@7g1FV/nQvEFisORFV@QOM2MSHGhkvi9FV@SWBvFzXzZmRwOfwE)

```perl
#`[
 
Only search square numbers that have at least N digits;
smaller could not possibly match.
 
Only bother to use analytics for large N. Finesse takes longer than brute force for small N.
 
]
 
unit sub MAIN ($timer = False);
 
sub first-square (Int $n) {
    my @start = flat '1', '0', (2 ..^ $n)».base: $n;
 
    if $n > 10 { # analytics
        my $root  = digital-root( @start.join, :base($n) );
        my @roots = (2..$n).map(*²).map: { digital-root($_.base($n), :base($n) ) };
        if $root ∉ @roots {
            my $offset = min(@roots.grep: * > $root ) - $root;
            @start[1+$offset] = $offset ~ @start[1+$offset];
        }
    }
 
    my $start = @start.join.parse-base($n).sqrt.ceiling;
    my @digits = reverse (^$n)».base: $n;
    my $sq;
    my $now  = now;
    my $time = 0;
    my $sr;
    for $start .. * {
        $sq = .²;
        my $s = $sq.base($n);
        my $f;
        $f = 1 and last unless $s.contains: $_ for @digits;
        if $timer && $n > 19 && $_ %% 1_000_000 {
            $time += now - $now;
            say "N $n:  {$_}² = $sq <$s> : {(now - $now).round(.001)}s" ~
                " : {$time.round(.001)} elapsed";
            $now = now;
        }
        next if $f;
        $sr = $_;
        last
    }
    sprintf( "Base %2d: %13s² == %-30s", $n, $sr.base($n), $sq.base($n) ) ~
        ($timer ?? ($time + now - $now).round(.001) !! '');
}
 
sub digital-root ($root is copy, :$base = 10) {
    $root = $root.comb.map({:36($_)}).sum.base($base) while $root.chars > 1;
    $root.parse-base($base);
}
 
say  "First perfect square with N unique digits in base N: ";
say .&first-square for flat
   2 .. 12, # required
  13 .. 16, # optional
  17 .. 19, # stretch
  20, # slow
  21, # pretty fast
  22, # very slow
  23, # don't hold your breath
  24, # slow but not too terrible
  25, # very slow
  26, #   "
;
```

#### Output:
```
First perfect square with N unique digits in base N:
Base  2:            10² == 100
Base  3:            22² == 2101
Base  4:            33² == 3201
Base  5:           243² == 132304
Base  6:           523² == 452013
Base  7:          1431² == 2450361
Base  8:          3344² == 13675420
Base  9:         11642² == 136802574
Base 10:         32043² == 1026753849
Base 11:        111453² == 1240A536789
Base 12:        3966B9² == 124A7B538609
Base 13:       3828943² == 10254773CA86B9
Base 14:       3A9DB7C² == 10269B8C57D3A4
Base 15:      1012B857² == 102597BACE836D4
Base 16:      404A9D9B² == 1025648CFEA37BD9
Base 17:     423F82GA9² == 101246A89CGFB357ED
Base 18:     44B482CAD² == 10236B5F8EG4AD9CH7
Base 19:    1011B55E9A² == 10234DHBG7CI8F6A9E5
Base 20:    49DGIH5D3G² == 1024E7CDI3HB695FJA8G
Base 21:   4C9HE5FE27F² == 1023457DG9HI8J6B6KCEAF
Base 22:   4F94788GJ0F² == 102369FBGDEJ48CHI7LKA5
Base 23:  1011D3EL56MC² == 10234ACEDKG9HM8FBJIL756
Base 24:  4LJ0HDGF0HD3² == 102345B87HFECKJNIGMDLA69
Base 25: 1011E145FHGHM² == 102345DOECKJ6GFB8LIAM7NH9
Base 26: 52K8N53BDM99K² == 1023458LO6IEMKG79FPCHNJDBA
```
