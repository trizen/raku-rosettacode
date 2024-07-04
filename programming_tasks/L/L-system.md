[1]: https://rosettacode.org/wiki/L-system

# [L-system][1]

L-system functionality as a Role that may be mixed in to a scalar.

```perl
# L-system functionality 
role Lindenmayer {
    has %.rules;
    method succ {
        self.comb.map( { %!rules{$^c} // $c } ).join but Lindenmayer(%!rules)
    }
}

# Testing
my $rabbits = 'I' but Lindenmayer({I => 'M', M => 'MI'});

.say for $rabbits++ xx 6;
```

#### Output:
```
I
M
MI
MIM
MIMMI
MIMMIMIM
```


Also see:

[Dragon_curve#Raku](https://rosettacode.org/wiki/Dragon_curve#Raku)

[Hilbert_curve#Raku](https://rosettacode.org/wiki/Hilbert_curve#Raku)

[Koch_curve#Raku](https://rosettacode.org/wiki/Koch_curve#Raku)

[Peano_curve#Raku](https://rosettacode.org/wiki/Peano_curve#Raku)

[Penrose_tiling#Raku](https://rosettacode.org/wiki/Penrose_tiling#Raku)

[Sierpinski_curve#Raku](https://rosettacode.org/wiki/Sierpinski_curve#Raku)

[Sierpinski_arrowhead_curve#Raku](https://rosettacode.org/wiki/Sierpinski_arrowhead_curve#Raku)

[Sierpinski_square_curve#Raku](https://rosettacode.org/wiki/Sierpinski_square_curve#Raku)

among others...
