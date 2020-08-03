[1]: https://rosettacode.org/wiki/Pisano_period

# [Pisano period][1]




Didn't bother making two differently named routines, just made a multi that will auto dispatch to the correct candidate.

```raku
use Prime::Factor;
 
constant @fib := 1,1,*+*…*;
 
my %cache;
 
multi pisano-period (Int $p where *.is-prime, Int $k where * > 0 = 1) {
    return %cache{"$p|$k"} if %cache{"$p|$k"};
    my $fibmod = @fib.map: * % $p**$k;
    %cache{"$p|$k"} = (1..*).first: { !$fibmod[$_-1] and ($fibmod[$_] == 1) }
}
 
multi pisano-period (Int $p where * > 0 ) {
    [lcm] prime-factors($p).Bag.map: { samewith .key, .value }
}
 
 
put "Pisano period (p, 2) for primes less than 50";
put (map { pisano-period($_, 2) }, ^50 .grep: *.is-prime )».fmt('%4d');
 
put "\nPisano period (p, 1) for primes less than 180";
.put for (map { pisano-period($_, 1) }, ^180 .grep: *.is-prime )».fmt('%4d').batch(15);
 
put "\nPisano period (p, 1) for integers 1 to 180";
.put for (1..180).map( { pisano-period($_) } )».fmt('%4d').batch(15);
```

#### Output:
```
Pisano period (p, 2) for primes less than 50
   6   24  100  112  110  364  612  342 1104  406  930 2812 1640 3784 1504

Pisano period (p, 1) for primes less than 180
   3    8   20   16   10   28   36   18   48   14   30   76   40   88   32
 108   58   60  136   70  148   78  168   44  196   50  208   72  108   76
 256  130  276   46  148   50  316  328  336  348  178

Pisano period (p, 1) for integers 1 to 180
   1    3    8    6   20   24   16   12   24   60   10   24   28   48   40
  24   36   24   18   60   16   30   48   24  100   84   72   48   14  120
  30   48   40   36   80   24   76   18   56   60   40   48   88   30  120
  48   32   24  112  300   72   84  108   72   20   48   72   42   58  120
  60   30   48   96  140  120  136   36   48  240   70   24  148  228  200
  18   80  168   78  120  216  120  168   48  180  264   56   60   44  120
 112   48  120   96  180   48  196  336  120  300   50   72  208   84   80
 108   72   72  108   60  152   48   76   72  240   42  168  174  144  120
 110   60   40   30  500   48  256  192   88  420  130  120  144  408  360
  36  276   48   46  240   32  210  140   24  140  444  112  228  148  600
  50   36   72  240   60  168  316   78  216  240   48  216  328  120   40
 168  336   48  364  180   72  264  348  168  400  120  232  132  178  120
```
