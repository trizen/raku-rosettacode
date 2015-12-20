[1]: http://rosettacode.org/wiki/Gray_code

# [Gray code][1]

```perl6
sub gray_encode ( Int $n --> Int ) {
    return $n +^ ( $n +> 1 );
}
 
sub gray_decode ( Int $n is copy --> Int ) {
    my $mask = 1 +< (32-2);
    $n +^= $mask +> 1 if $n +& $mask while $mask +>= 1;
    return $n;
}
 
for ^32 -> $n {
    my $g = gray_encode($n);
    my $d = gray_decode($g);
    printf "%2d: %5b => %5b => %5b: %2d\n", $n, $n, $g, $d, $d;
    die if $d != $n;
}
```


This version is a translation of the Haskell solution,
and produces the same output as the first Perl 6 solution.

```perl6
multi bin_to_gray ( [] ) { [] }
multi bin_to_gray ( [$head, *@tail] ) {
    return [ $head, ( @tail Z+^ ($head, @tail) ) ];
}
 
multi gray_to_bin ( [] ) { [] }
multi gray_to_bin ( [$head, *@tail] ) {
    my @bin := $head, (@tail Z+^ @bin);
    return @bin.flat;
}
 
for ^32 -> $n {
    my @b = $n.fmt('%b').comb;
    my $g = bin_to_gray(@b);
    my $d = gray_to_bin($g);
    printf "%2d: %5s => %5s => %5s: %2d\n",
            $n, @b.join, $g.join, $d.join, :2($d.join);
    die if :2($d.join) != $n;
}
```

#### Output:
```
 0:     0 =>     0 =>     0:  0
 1:     1 =>     1 =>     1:  1
 2:    10 =>    11 =>    10:  2
 3:    11 =>    10 =>    11:  3
 4:   100 =>   110 =>   100:  4
 5:   101 =>   111 =>   101:  5
 6:   110 =>   101 =>   110:  6
 7:   111 =>   100 =>   111:  7
 8:  1000 =>  1100 =>  1000:  8
 9:  1001 =>  1101 =>  1001:  9
10:  1010 =>  1111 =>  1010: 10
11:  1011 =>  1110 =>  1011: 11
12:  1100 =>  1010 =>  1100: 12
13:  1101 =>  1011 =>  1101: 13
14:  1110 =>  1001 =>  1110: 14
15:  1111 =>  1000 =>  1111: 15
16: 10000 => 11000 => 10000: 16
17: 10001 => 11001 => 10001: 17
18: 10010 => 11011 => 10010: 18
19: 10011 => 11010 => 10011: 19
20: 10100 => 11110 => 10100: 20
21: 10101 => 11111 => 10101: 21
22: 10110 => 11101 => 10110: 22
23: 10111 => 11100 => 10111: 23
24: 11000 => 10100 => 11000: 24
25: 11001 => 10101 => 11001: 25
26: 11010 => 10111 => 11010: 26
27: 11011 => 10110 => 11011: 27
28: 11100 => 10010 => 11100: 28
29: 11101 => 10011 => 11101: 29
30: 11110 => 10001 => 11110: 30
31: 11111 => 10000 => 11111: 31
```


Perl 6 distinguishes numeric bitwise operators with a leading <tt>+</tt> sign,
so <tt>+&lt;</tt> and <tt>+&gt;</tt> are left and right shift,
while <tt>+&amp;</tt> is a bitwise AND, while <tt>+^</tt> is bitwise XOR
(here used as part of an assignment metaoperator).