[1]: https://rosettacode.org/wiki/Juggler_sequence

# [Juggler sequence][1]

Reaches 30817 fairly quickly but later values suck up enough memory that it starts thrashing the disk cache and performance drops off a cliff (on my system). Killed it after 10 minutes and capped list at 30817. Could rewrite to not try to hold entire sequence in memory at once, but probably not worth it. If you want sheer numeric calculation performance, Raku is probably not where it's at.

```perl
use Lingua::EN::Numbers;
sub juggler (Int $n where * > 0) { $n, { $_ +& 1 ?? .³.&isqrt !! .&isqrt } … 1 }
 
sub isqrt ( \x ) { my ( $X, $q, $r, $t ) =  x, 1, 0 ;
    $q +<= 2 while $q ≤ $X ;
    while $q > 1 {
        $q +>= 2; $t = $X - $r - $q; $r +>= 1;
        if $t ≥ 0 { $X = $t; $r += $q }
    }
    $r
}
 
say " n  l[n]  i[n]   h[n]";
for 20..39 {
    my @j = .&juggler;
    my $max = @j.max;
    printf "%2s %4d  %4d    %s\n", .&comma, +@j-1, @j.first(* == $max, :k), comma $max;
}
 
say "\n      n     l[n]   i[n]    d[n]";
( 113, 173, 193, 2183, 11229, 15065, 15845, 30817 ).hyper(:1batch).map: {
    my $start = now;
    my @j = .&juggler;
    my $max = @j.max;
    printf "%10s %4d   %4d %10s   %6.2f seconds\n", .&comma, +@j-1, @j.first(* == $max, :k),
      $max.chars.&comma, (now - $start);
}
```

#### Output:
```
 n  l[n]  i[n]   h[n]
20    3     0    20
21    9     4    140
22    3     0    22
23    9     1    110
24    3     0    24
25   11     3    52,214
26    6     3    36
27    6     1    140
28    6     3    36
29    9     1    156
30    6     3    36
31    6     1    172
32    6     3    36
33    8     2    2,598
34    6     3    36
35    8     2    2,978
36    3     0    36
37   17     8    24,906,114,455,136
38    3     0    38
39   14     3    233,046

      n     l[n]   i[n]    d[n]
       113   16      9         27     0.01 seconds
       173   32     17         82     0.01 seconds
       193   73     47        271     0.09 seconds
     2,183   72     32      5,929     1.05 seconds
    11,229  101     54      8,201     1.98 seconds
    15,065   66     25     11,723     2.05 seconds
    15,845  139     43     23,889    10.75 seconds
    30,817   93     39     45,391    19.60 seconds
```
