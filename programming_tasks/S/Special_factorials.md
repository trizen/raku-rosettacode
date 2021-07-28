[1]: https://rosettacode.org/wiki/Special_factorials

# [Special factorials][1]

```perl
sub postfix:<!> ($n) {   [*] 1 .. $n }
sub postfix:<$> ($n) { [R**] 1 .. $n }
 
sub sf ($n) { [*] map {                      $_! }, 1 .. $n }
sub H  ($n) { [*] map {                $_ ** $_  }, 1 .. $n }
sub af ($n) { [+] map { (-1) ** ($n - $_) *  $_! }, 1 .. $n }
sub rf ($n) {
    state @f = 1, |[\*] 1..*;
    $n == .value ?? .key !! Nil given @f.first: :p, * >= $n;
}
 
say 'sf : ', map &sf , 0..9;
say 'H  : ', map &H  , 0..9;
say 'af : ', map &af , 0..9;
say '$  : ', map *$  , 1..4;
 
say '5$ has ', 5$.chars, ' digits';
 
say 'rf : ', map &rf, 1, 2, 6, 24, 120, 720, 5040, 40320, 362880, 3628800;
say 'rf(119) = ', rf(119).raku;
```

#### Output:
```
sf : (1 1 2 12 288 34560 24883200 125411328000 5056584744960000 1834933472251084800000)
H  : (1 1 4 108 27648 86400000 4031078400000 3319766398771200000 55696437941726556979200000 21577941222941856209168026828800000)
af : (0 1 1 5 19 101 619 4421 35899 326981)
$  : (1 2 9 262144)
5$ has 183231 digits
rf : (0 2 3 4 5 6 7 8 9 10)
rf(119) = Nil
```
