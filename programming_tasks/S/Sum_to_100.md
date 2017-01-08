[1]: http://rosettacode.org/wiki/Sum_to_100

# [Sum to 100][1]

```perl
my @ops = ['-', ''], |( [' + ', ' - ', ''] xx 8 );
my @str = [X~] map { .Slip }, ( @ops Z 1..9 );
my %sol = @str.classify: *.subst( ' - ', ' -', :g )\
                          .subst( ' + ',  ' ', :g ).words.sum;
 
my %count.push: %sol.map({ .value.elems => .key });
 
my $max_solutions    = %count.max( + *.key );
my $first_unsolvable = first { %sol{$_} :!exists }, 1..*;
my @two_largest_sums = %sol.keys.sort(-*)[^2];
 
given %sol{100}:p {
    say "{.value.elems} solutions for sum {.key}:";
    say "    $_" for .value.list;
}
 
say .perl for :$max_solutions, :$first_unsolvable, :@two_largest_sums;
```

#### Output:
```
12 solutions for sum 100:
    -1 + 2 - 3 + 4 + 5 + 6 + 78 + 9
    1 + 2 + 3 - 4 + 5 + 6 + 78 + 9
    1 + 2 + 34 - 5 + 67 - 8 + 9
    1 + 23 - 4 + 5 + 6 + 78 - 9
    1 + 23 - 4 + 56 + 7 + 8 + 9
    12 + 3 + 4 + 5 - 6 - 7 + 89
    12 + 3 - 4 + 5 + 67 + 8 + 9
    12 - 3 - 4 + 5 - 6 + 7 + 89
    123 + 4 - 5 + 67 - 89
    123 + 45 - 67 + 8 - 9
    123 - 4 - 5 - 6 - 7 + 8 - 9
    123 - 45 - 67 + 89
:max_solutions("46" => $["9", "-9"])
:first_unsolvable(211)
:two_largest_sums(["123456789", "23456790"])
```