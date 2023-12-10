[1]: https://rosettacode.org/wiki/Almkvist-Giullera_formula_for_pi

# [Almkvist-Giullera formula for pi][1]

```perl
# 20201013 Raku programming solution

use BigRoot;
use Rat::Precise;
use experimental :cached;

BigRoot.precision = 75 ;
my $Precision     = 70 ;
my $AGcache       =  0 ;

sub postfix:<!>(Int $n --> Int) is cached { [*] 1 .. $n }

sub Integral(Int $n --> Int) is cached {
   (2⁵*(6*$n)! * (532*$n² + 126*$n + 9)) div (3*($n!)⁶)
}

sub A-G(Int $n --> FatRat) is cached { # Almkvist-Giullera
   Integral($n).FatRat / (10**(6*$n + 3)).FatRat
}

sub Pi(Int $n --> Str) {
   (1/(BigRoot.newton's-sqrt: $AGcache += A-G $n)).precise($Precision)
}

say "First 10 integer portions : ";
say $_, "\t", Integral $_ for ^10;

my $target = Pi my $Nth = 0;

loop { $target eq ( my $next = Pi ++$Nth ) ?? ( last ) !! $target = $next }

say "π to $Precision decimal places is :\n$target"
```

#### Output:
```
First 10 integer portions :
0       96
1       5122560
2       190722470400
3       7574824857600000
4       312546150372456000000
5       13207874703225491420651520
6       567273919793089083292259942400
7       24650600248172987140112763715584000
8       1080657854354639453670407474439566400000
9       47701779391594966287470570490839978880000000
π to 70 decimal places is :
3.1415926535897932384626433832795028841971693993751058209749445923078164
```
