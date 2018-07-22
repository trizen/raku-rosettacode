[1]: https://rosettacode.org/wiki/Van_der_Corput_sequence

# [Van der Corput sequence][1]

First a cheap implementation in base 2, using string operations.

```perl
constant VdC = map { :2("0." ~ .base(2).flip) }, ^Inf;
.say for VdC[^16];
```


Here is a more elaborate version using the polymod built-in integer method:

```perl
sub VdC($base = 2) {
    map {
        [+] $_ && .polymod($base xx *) Z/ [\*] $base xx *
    }, ^Inf
}
 
.say for VdC[^10];
```

#### Output:
```
0
0.5
0.25
0.75
0.125
0.625
0.375
0.875
0.0625
0.5625
```


Here is a fairly standard imperative version in which we mutate three variables in parallel:

```perl
sub vdc($num, $base = 2) {
    my $n = $num;
    my $vdc = 0;
    my $denom = 1;
    while $n {
        $vdc += $n mod $base / ($denom *= $base);
        $n div= $base;
    }
    $vdc;
}
 
for 2..5 -> $b {
    say "Base $b";
    say (vdc($_,$b) for ^10).perl;
    say '';
}
```

#### Output:
```
Base 2
(0, 1/2, 1/4, 3/4, 1/8, 5/8, 3/8, 7/8, 1/16, 9/16)

Base 3
(0, 1/3, 2/3, 1/9, 4/9, 7/9, 2/9, 5/9, 8/9, 1/27)

Base 4
(0, 1/4, 1/2, 3/4, 1/16, 5/16, 9/16, 13/16, 1/8, 3/8)

Base 5
(0, 1/5, 2/5, 3/5, 4/5, 1/25, 6/25, 11/25, 16/25, 21/25)
```


Here is a functional version that produces the same output:

```perl
sub vdc($value, $base = 2) {
    my @values = $value, { $_ div $base } ... 0;
    my @denoms = $base,  { $_  *  $base } ... *;
    [+] do for (flat @values Z @denoms) -> $v, $d {
        $v mod $base / $d;
    }
}
```


We first define two sequences, one finite, one infinite.
When we zip those sequences together, the finite sequence terminates the loop (which, since a Perl&#160;6 loop returns all its values, is merely another way of writing a `map`).
We then sum with `[+]`, a reduction of the `+` operator.
(We could have in-lined the sequences or used a traditional `map` operator, but this way seems more readable than the typical FP solution.)
The `do` is necessary to introduce a statement where a term is expected, since Perl&#160;6 distinguishes "sentences" from "noun phrases" as a natural language might.