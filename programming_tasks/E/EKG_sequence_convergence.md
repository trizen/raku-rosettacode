[1]: https://rosettacode.org/wiki/EKG_sequence_convergence

# [EKG sequence convergence][1]

```perl
sub infix:<shares-divisors-with> { ($^a gcd $^b) > 1 }
 
sub next-EKG ( *@s ) {
    return first {
        @s ∌ $_  and  @s.tail shares-divisors-with $_
    }, 2..*;
}
 
sub EKG ( Int $start ) {  1, $start, &next-EKG … *  }
 
sub converge-at ( @ints ) {
    my @ekgs = @ints.map: &EKG;
 
    return (2 .. *).first: -> $i {
        [==]  @ekgs.map(     *.[$i]     ) and
        [===] @ekgs.map( *.head($i).Set )
    }
}
 
say "EKG($_): ", .&EKG.head(10) for 2, 5, 7, 9, 10;
 
for [5, 7], [2, 5, 7, 9, 10] -> @ints {
    say "EKGs of (@ints[]) converge at term {$_+1}" with converge-at(@ints);
}
```

#### Output:
```
EKG(2): (1 2 4 6 3 9 12 8 10 5)
EKG(5): (1 5 10 2 4 6 3 9 12 8)
EKG(7): (1 7 14 2 4 6 3 9 12 8)
EKG(9): (1 9 3 6 2 4 8 10 5 15)
EKG(10): (1 10 2 4 6 3 9 12 8 14)
EKGs of (5 7) converge at term 21
EKGs of (2 5 7 9 10) converge at term 45
```