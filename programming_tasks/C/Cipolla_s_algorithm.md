[1]: http://rosettacode.org/wiki/Cipolla's_algorithm

# [Cipolla's algorithm][1]

```perl
#  Legendre operator (𝑛│𝑝)
sub infix:<│> (Int \𝑛, Int \𝑝 where 𝑝.is-prime && (𝑝 != 2)) {
    given 𝑛.expmod( (𝑝-1) div 2, 𝑝 ) {
        when 0  {  0 }
        when 1  {  1 }
        default { -1 }
    }
}
 
# a coordinate in a Field of p elements
class Fp {
    has Int $.x;
    has Int $.y;
}
 
sub cipolla ( Int \𝑛, Int \𝑝 ) {
    note "Invalid parameters ({𝑛}, {𝑝})"
      and return Nil if (𝑛│𝑝) != 1;
    my $ω2;
    my $a = 0;
    loop {
        last if ($ω2 = ($a² - 𝑛) % 𝑝)│𝑝 < 0;
        $a++;
    }
 
    # define a local multiply operator for Field coordinates
    multi sub infix:<*> ( Fp $a, Fp $b ){
        Fp.new: :x(($a.x * $b.x + $a.y * $b.y * $ω2) % 𝑝),
                :y(($a.x * $b.y + $b.x * $a.y)       % 𝑝)
    }
 
    my $r = Fp.new: :x(1),  :y(0);
    my $s = Fp.new: :x($a), :y(1);
 
    for (𝑝+1) +> 1, * +> 1 ... 1 {
        $r *= $s if $_ % 2;
        $s *= $s;
    }
    return Nil if $r.y;
    $r.x;
}
 
my @tests = (
    (10, 13),
    (56, 101),
    (8218, 10007),
    (8219, 10007),
    (331575, 1000003),
    (665165880, 1000000007),
    (881398088036, 1000000000039),
    (34035243914635549601583369544560650254325084643201,
      100000000000000000000000000000000000000000000000151)
);
 
for @tests -> ($n, $p) {
   my $r = cipolla($n, $p);
   say $r ?? "Roots of $n are ($r, {$p-$r}) mod $p"
          !! "No solution for ($n, $p)"
}
 
```

#### Output:
```
Roots of 10 are (6, 7) mod 13
Roots of 56 are (37, 64) mod 101
Roots of 8218 are (9872, 135) mod 10007
Invalid parameters (8219, 10007)
No solution for (8219, 10007)
Roots of 331575 are (855842, 144161) mod 1000003
Roots of 665165880 are (475131702, 524868305) mod 1000000007
Roots of 881398088036 are (791399408049, 208600591990) mod 1000000000039
Roots of 34035243914635549601583369544560650254325084643201 are (82563118828090362261378993957450213573687113690751, 17436881171909637738621006042549786426312886309400) mod 100000000000000000000000000000000000000000000000151
```