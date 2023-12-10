[1]: https://rosettacode.org/wiki/Determinant_and_permanent

# [Determinant and permanent][1]





Uses the permutations generator from the [Permutations by swapping](https://rosettacode.org/wiki/Permutations_by_swapping#Raku) task. This implementation is naive and brute-force (slow) but exact.

```perl
sub insert ($x, @xs) { ([flat @xs[0 ..^ $_], $x, @xs[$_ .. *]] for 0 .. @xs) }
sub order ($sg, @xs) { $sg > 0 ?? @xs !! @xs.reverse }

multi σ_permutations ([]) { [] => 1 }

multi σ_permutations ([$x, *@xs]) {
    σ_permutations(@xs).map({ |order($_.value, insert($x, $_.key)) }) Z=> |(1,-1) xx *
}

sub m_arith ( @a, $op ) {
    note "Not a square matrix" and return
      if [||] map { @a.elems cmp @a[$_].elems }, ^@a;
    sum σ_permutations([^@a]).race.map: {
        my $permutation = .key;
        my $term = $op eq 'perm' ?? 1 !! .value;
        for $permutation.kv -> $i, $j { $term *= @a[$i][$j] };
        $term
    }
}

######### helper subs #########
sub hilbert-matrix (\h) {[(1..h).map(-> \n {[(n..^n+h).map: {(1/$_).FatRat}]})]}

sub rat-or-int ($num) {
    return $num unless $num ~~ Rat|FatRat;
    return $num.narrow if $num.narrow.WHAT ~~ Int;
    $num.nude.join: '/';
}

sub say-it ($message, @array) {
    my $max;
    @array.map: {$max max= max $_».&rat-or-int.comb(/\S+/)».chars};
    say "\n$message";
    $_».&rat-or-int.fmt(" %{$max}s").put for @array;
}

########### Testing ###########
my @tests = (
    [
        [ 1, 2 ],
        [ 3, 4 ]
    ],
    [
        [  1,  2,  3,  4 ],
        [  4,  5,  6,  7 ],
        [  7,  8,  9, 10 ],
        [ 10, 11, 12, 13 ]
    ],
    hilbert-matrix 7
);

for @tests -> @matrix {
    say-it 'Matrix:', @matrix;
    say "Determinant:\t", rat-or-int @matrix.&m_arith: <det>;
    say "Permanent:  \t", rat-or-int @matrix.&m_arith: <perm>;
    say '-' x 40;
}
```


**Output**


```
Matrix:
 1  2
 3  4
Determinant:    -2
Permanent:      10
----------------------------------------

Matrix:
  1   2   3   4
  4   5   6   7
  7   8   9  10
 10  11  12  13
Determinant:    0
Permanent:      29556
----------------------------------------

Matrix:
    1   1/2   1/3   1/4   1/5   1/6   1/7
  1/2   1/3   1/4   1/5   1/6   1/7   1/8
  1/3   1/4   1/5   1/6   1/7   1/8   1/9
  1/4   1/5   1/6   1/7   1/8   1/9  1/10
  1/5   1/6   1/7   1/8   1/9  1/10  1/11
  1/6   1/7   1/8   1/9  1/10  1/11  1/12
  1/7   1/8   1/9  1/10  1/11  1/12  1/13
Determinant:    1/2067909047925770649600000
Permanent:      29453515169174062608487/2067909047925770649600000
----------------------------------------
```
