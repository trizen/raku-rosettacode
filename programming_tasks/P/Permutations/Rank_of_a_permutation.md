[1]: https://rosettacode.org/wiki/Permutations/Rank_of_a_permutation

# [Permutations/Rank of a permutation][1]





It is similar to Haskell, but separate something like [inversion vector](https://en.wikipedia.org/wiki/Inversion_(discrete_mathematics)).
It is easy generate random inversion vector without BigInt.

```perl
use v6;

sub rank2inv ( $rank, $n = * ) {
    $rank.polymod( 1 ..^ $n );
}

sub inv2rank ( @inv ) {
    [+] @inv Z* [\*] 1, 1, * + 1 ... * 
}

sub inv2perm ( @inv, @items is copy = ^@inv.elems ) {
    my @perm;
    for @inv.reverse -> $i {
        @perm.append: @items.splice: $i, 1;
    }
    @perm;
}

sub perm2inv ( @perm ) {     #not in linear time 
    (
        { @perm[++$ .. *].grep( * < $^cur ).elems } for @perm;  
    ).reverse;
}    
    
for ^6 {
    my @row.push: $^rank;
    for ( *.&rank2inv(3) , &inv2perm, &perm2inv, &inv2rank )  -> &code {
        @row.push: code( @row[*-1] );
    }
    say @row;
}

my $perms =  4;      #100;
my $n     = 12;      #144;

say 'Via BigInt rank';
for ( ( ^([*] 1 .. $n) ).pick($perms) ) {
    say $^rank.&rank2inv($n).&inv2perm; 
};

say 'Via inversion vectors';
for ( { my $i=0;  inv2perm (^++$i).roll xx $n } ... *  ).unique( with => &[eqv] ).[^$perms] {
    .say;
};

say 'Via Raku method pick';
for ( { [(^$n).pick(*)] } ... * ).unique( with => &[eqv] ).head($perms) {
    .say
};
```

#### Output:
```
[0 (0 0 0) [0 1 2] (0 0 0) 0]
[1 (0 1 0) [0 2 1] (0 1 0) 1]
[2 (0 0 1) [1 0 2] (0 0 1) 2]
[3 (0 1 1) [1 2 0] (0 1 1) 3]
[4 (0 0 2) [2 0 1] (0 0 2) 4]
[5 (0 1 2) [2 1 0] (0 1 2) 5]
Via BigInt rank
[4 3 1 8 6 2 0 7 9 11 5 10]
[0 8 11 4 9 3 7 5 2 6 10 1]
[5 7 9 11 10 6 4 1 2 3 0 8]
[9 11 8 6 3 5 7 2 4 0 1 10]
Via inversion vectors
[9 0 3 1 8 2 4 5 11 7 10 6]
[7 3 1 10 0 6 4 11 2 9 8 5]
[9 8 5 11 1 10 0 7 4 6 2 3]
[10 8 6 5 4 9 0 2 11 7 1 3]
Via Raku method pick
[11 0 7 10 9 4 1 8 6 5 2 3]
[4 5 8 3 2 1 7 9 11 0 10 6]
[11 7 9 4 0 8 10 1 5 2 6 3]
[11 10 0 3 4 6 7 9 8 5 1 2]
```
