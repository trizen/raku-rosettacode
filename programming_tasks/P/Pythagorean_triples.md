[1]: https://rosettacode.org/wiki/Pythagorean_triples

# [Pythagorean triples][1]

Here is a straight-forward, naive brute force implementation:

```raku
constant limit = 100;
 
for [X] [^limit] xx 3 -> (\a, \b, \c) {
    say [a, b, c] if a < b < c and a**2 + b**2 == c**2
}
```


Here is a slightly less naive brute force implementation that is not really practical for large perimeter limits. It's pretty zippy up to about 10000 though.

```raku
my %triples;
my $limit = 10000;
 
for 3 .. $limit/2 -> $c {
   for 1 .. $c -> $a {
       my $b = ($c * $c - $a * $a).sqrt;
       last if $c + $a + $b > $limit;
       last if $a > $b;
       if $b == $b.Int {
          my $key = "$a $b $c";
          %triples{$key} = ([gcd] $c, $a, $b.Int) > 1 ?? 0  !! 1;
          say $key, %triples{$key} ?? ' - primitive' !! '';
       }
   } 
}
 
say "There are {+%triples.keys} Pythagorean Triples with a perimeter <= $limit,"
 ~"\nof which {[+] %triples.values} are primitive.";
```

#### Output:
```
3 4 5 - primitive
6 8 10
5 12 13 - primitive
9 12 15
8 15 17 - primitive
12 16 20
7 24 25 - primitive
15 20 25
10 24 26
20 21 29 - primitive
...
196 4800 4804
310 4800 4810
392 4794 4810
171 4872 4875
99 4900 4901 - primitive
140 4899 4901 - primitive
There are 4858 Pythagorean Triples with a perimeter <= 10000,
of which 703 are primitive.
```


Here's a much faster version. Hint, "oyako" is Japanese for "parent/child". `:-)`

```raku
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


#### Output:
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


The geometric sequence of limits will continue on forever, so eventually when you get tired of waiting (about a billion on my computer), you can just stop it. Another efficiency trick of note: we avoid passing the limit in as a parameter to the inner helper routine, but instead supply the limit via the lexical scope. Likewise, the accumulators are referenced lexically, so only the triples themselves need to be passed downward, and nothing needs to be returned.



Here is an alternate version that avoids naming any scalars that can be handled by vector processing instead:

```raku
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
 
for 10,100,1000 ... * -> $limit {
    say triples $limit;
}
```


Using vectorized ops allows a bit more potential for parallelization, though this is probably not as big a win in this case, especially since we do a certain amount of multiplying by 1 that the scalar version doesn't need to do.
Note the cute trick of adding complex numbers to add two numbers in parallel.
The use of `gather`/`take` allows the summation to run in a different thread than the helper function, at least in theory...



In practice, this solution runs considerably slower than the previous one, due primarily to passing `gather`/`take` values up many levels of dynamic scope. Eventually this may be optimized.