[1]: https://rosettacode.org/wiki/Pythagorean_triples

# [Pythagorean triples][1]





Here is a straight-forward, naïve brute force implementation:

```perl
constant limit = 100;

for [X] [^limit] xx 3 -> (\a, \b, \c) {
    say [a, b, c] if a < b < c and a + b + c <= limit and a*b + b*b == c*c
}
```

#### Output:
```
[3 4 5]
[5 12 13]
[6 8 10]
[7 24 25]
[8 15 17]
[9 12 15]
[9 40 41]
[10 24 26]
[12 16 20]
[12 35 37]
[15 20 25]
[15 36 39]
[16 30 34]
[18 24 30]
[20 21 29]
[21 28 35]
[24 32 40]
```


Here is a slightly less naive brute force implementation, but still not practical for large perimeter limits.

```perl
my $limit = 10000;
my atomicint $i = 0;
my @triples[$limit/2];
(3 .. $limit/2).race.map: -> $c {
   for 1 .. $c -> $a {
       my $b = ($c * $c - $a * $a).sqrt;
       last if $c + $a + $b > $limit;
       last if $a > $b;
       @triples[$i⚛++] = ([gcd] $c, $a, $b) > 1 ?? 0 !! 1 if $b == $b.Int;
   }
}

say my $result = "There are {+@triples.grep:{$_ !eqv Any}} Pythagorean Triples with a perimeter <= $limit,"
 ~"\nof which {[+] @triples.grep: so *} are primitive.";
```

#### Output:
```
There are 4858 Pythagorean Triples with a perimeter <= 10000,
of which 703 are primitive.
```


Here's a much faster version.  Hint, "oyako" is Japanese for "parent/child". `:-)`

```perl
sub triples($limit) {
    my $primitive = 0;
    my $civilized = 0;
 
    sub oyako($a, $b, $c) {
        my $perim = $a + $b + $c;
        return if $perim > $limit;
        ++$primitive; $civilized += $limit div $perim;
        oyako( $a - 2*$b + 2*$c,  2*$a - $b + 2*$c,  2*$a - 2*$b + 3*$c);
        oyako( $a + 2*$b + 2*$c,  2*$a + $b + 2*$c,  2*$a + 2*$b + 3*$c);
        oyako(-$a + 2*$b + 2*$c, -2*$a + $b + 2*$c, -2*$a + 2*$b + 3*$c);
    }
 
    oyako(3,4,5);
    "$limit => ($primitive $civilized)";
}
 
for 10,100,1000 ... * -> $limit {
    say triples $limit;
}
```


Output:


```
10 => (0 0)
100 => (7 17)
1000 => (70 325)
10000 => (703 4858)
100000 => (7026 64741)
1000000 => (70229 808950)
10000000 => (702309 9706567)
100000000 => (7023027 113236940)
1000000000 => (70230484 1294080089)
^C
```


The geometric sequence of limits will continue on forever, so eventually when you get tired of waiting (about a billion on my computer), you can just stop it.  Another efficiency trick of note: we avoid passing the limit in as a parameter to the inner helper routine, but instead supply the limit via the lexical scope.  Likewise, the accumulators are referenced lexically, so only the triples themselves need to be passed downward, and nothing needs to be returned.



Here is an alternate version that avoids naming any scalars that can be handled by vector processing instead. Using vectorized ops allows a bit more potential for parallelization in theory, but techniques like the use of complex numbers to add two numbers in parallel, and the use of `gather`/`take` generate so much overhead that this version runs 70-fold slower than the previous one.

```perl
constant @coeff = [[+1, -2, +2], [+2, -1, +2], [+2, -2, +3]],
                  [[+1, +2, +2], [+2, +1, +2], [+2, +2, +3]],
                  [[-1, +2, +2], [-2, +1, +2], [-2, +2, +3]];

sub triples($limit) {

    sub oyako(@trippy) {
        my $perim = [+] @trippy;
        return if $perim > $limit;
        take (1 + ($limit div $perim)i);
        for @coeff -> @nine {
            oyako (map -> @three { [+] @three »*« @trippy }, @nine);
        }
        return;
    }

    my $complex = 0i + [+] gather oyako([3,4,5]);
    "$limit => ({$complex.re, $complex.im})";
}

for 10, 100, 1000, 10000 -> $limit {
    say triples $limit;
}
```

#### Output:
```
10 => (0 0)
100 => (7 17)
1000 => (70 325)
10000 => (703 4858)
```
