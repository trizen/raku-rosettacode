[1]: http://rosettacode.org/wiki/Matrix_transposition

# [Matrix transposition][1]

```perl
sub transpose(@m)
{
    my @t;
    for ^@m X ^@m[0] -> ($x, $y) { @t[$y][$x] = @m[$x][$y] }
    return @t;
}
 
# creates a random matrix
my @a;
for (^10).pick X (^10).pick -> ($x, $y) { @a[$x][$y] = (^100).pick; }
 
say "original: ";
.perl.say for @a;
 
my @b = transpose(@a);
 
say "transposed: ";
.perl.say for @b;
```


A more concise solution:

```perl
sub transpose (@m) {
    ([ @m[*;$_] ] for ^@m[0]);
}
 
my @a = < a b c d e >,
        < f g h i j >,
        < k l m n o >,
        < p q r s t >;
 
.say for @a.&transpose;
```

#### Output:
```
[a f k p]
[b g l q]
[c h m r]
[d i n s]
[e j o t]
```


Using the <tt>[Z]</tt> meta-operator.

```perl
say [Z] (<A B C>,<D E F>,<G H I>)
```

#### Output:
```
((A D G) (B E H) (C F I))
```