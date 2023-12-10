[1]: https://rosettacode.org/wiki/Sum_to_100

# [Sum to 100][1]



```perl
my $sum = 100;
my $N   = 10;
my @ops = ['-', ''], |( [' + ', ' - ', ''] xx 8 );
my @str = [X~] map { .Slip }, ( @ops Z 1..9 );
my %sol = @str.classify: *.subst( ' - ', ' -', :g )\
                          .subst( ' + ',  ' ', :g ).words.sum;

my %count.push: %sol.map({ .value.elems => .key });

my $max-solutions    = %count.max( + *.key );
my $first-unsolvable = first { %sol{$_} :!exists }, 1..*;
sub n-largest-sums (Int $n) { %sol.sort(-*.key)[^$n].fmt: "%8s => %s\n" }

given %sol{$sum}:p {
    say "{.value.elems} solutions for sum {.key}:";
    say "    $_" for .value.list;
}

.say for :$max-solutions, :$first-unsolvable, "$N largest sums:", n-largest-sums($N);
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
max-solutions => 46 => [-9 9]
first-unsolvable => 211
10 largest sums:
123456789 => 123456789
 23456790 => 1 + 23456789
 23456788 => -1 + 23456789
 12345687 => 12345678 + 9
 12345669 => 12345678 - 9
  3456801 => 12 + 3456789
  3456792 => 1 + 2 + 3456789
  3456790 => -1 + 2 + 3456789
  3456788 => 1 - 2 + 3456789
  3456786 => -1 - 2 + 3456789
```
