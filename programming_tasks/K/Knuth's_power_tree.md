[1]: https://rosettacode.org/wiki/Knuth's_power_tree

# [Knuth's power tree][1]

Paths are random. It is possible replace `.pick(*)` with `.reverse` if you want paths as in Perl, or remove it for Python paths.

```perl
use v6;
 
sub power-path ($n ) {
    state @unused_nodes = (2,);
    state @power-tree = (False,0,1);
 
    until @power-tree[$n].defined {
        my $node = @unused_nodes.shift;
 
        for  $node X+ power-path($node).pick(*) {
            next if @power-tree[$_].defined;
            @unused_nodes.push($_);
            @power-tree[$_]= $node;
        }        
    }
 
    ( $n, { @power-tree[$_] } ...^ 0 ).reverse;
}
 
multi power ( $, 0 ) { 1 };
multi power ( $n, $exponent ) {
    state  %p;
    my     %r =  %p{$n}  // ( 0 => 1, 1 => $n ) ;  
 
    for power-path( $exponent ).rotor( 2 => -1 ) -> ( $p, $c ) {
        %r{ $c } = %r{ $p } * %r{ $c - $p }
    }
 
    %p{$n} := %r ;
    %r{ $exponent }
}
 
say 'Power paths: ',      pairs map *.&power-path,    ^18;
say '2 ** key = value: ', pairs map { 2.&power($_) }, ^18; 
 
say 'Path for 191: ', power-path 191;
say '3 ** 191 = ',    power   3, 191;
say 'Path for 81: ',  power-path  81;
say '1.1 ** 81 = ',   power 1.1,  81;
 
```

#### Output:
```
Power paths: (0 => () 1 => (1) 2 => (1 2) 3 => (1 2 3) 4 => (1 2 4) 5 => (1 2 3 5) 6 => (1 2 3 6) 7 => (1 2 3 6 7) 8 => (1 2 4 8) 9 => (1 2 3 6 9) 10 => (1 2 3 5 10) 11 => (1 2 3 6 9 11) 12 => (1 2 3 6 12) 13 => (1 2 3 6 12 13) 14 => (1 2 3 6 12 14) 15 => (1 2 3 6 9 15) 16 => (1 2 4 8 16) 17 => (1 2 4 8 16 17))
2 ** key = value: (0 => 1 1 => 2 2 => 4 3 => 8 4 => 16 5 => 32 6 => 64 7 => 128 8 => 256 9 => 512 10 => 1024 11 => 2048 12 => 4096 13 => 8192 14 => 16384 15 => 32768 16 => 65536 17 => 131072)
Path for 191: (1 2 3 6 9 18 27 54 108 162 189 191)
3 ** 191 = 13494588674281093803728157396523884917402502294030101914066705367021922008906273586058258347
Path for 81: (1 2 3 6 9 18 27 54 81)
1.1 ** 81 = 2253.24023604401
```