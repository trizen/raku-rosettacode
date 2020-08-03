[1]: https://rosettacode.org/wiki/Index_finite_lists_of_positive_integers

# [Index finite lists of positive integers][1]

Here is a cheap solution using a base-11 encoding and string operations:

```perl
sub rank(*@n)      { :11(@n.join('A')) }
sub unrank(Int $n) { $n.base(11).split('A') }
 
say my @n = (^20).roll(12);
say my $n = rank(@n);
say unrank $n;
```

#### Output:
```
1 11 16 1 3 9 0 2 15 7 19 10
25155454474293912130094652799
1 11 16 1 3 9 0 2 15 7 19 10
```


Here is a bijective solution that does not use string operations.

```perl
multi infix:<rad> ()       { 0 }
multi infix:<rad> ($a)     { $a }
multi infix:<rad> ($a, $b) { $a * $*RADIX + $b }

multi expand(Int $n is copy, 1) { $n }
multi expand(Int $n is copy, Int $*RADIX) {
    my \RAD = $*RADIX;

    my @reversed-digits = gather while $n > 0 {
    take $n % RAD;
    $n div= RAD;
    }

    eager for ^RAD {
    [rad] reverse @reversed-digits[$_, * + RAD ... *]
    }
}

multi compress(@n where @n == 1) { @n[0] }
multi compress(@n is copy) {
    my \RAD = my $*RADIX = @n.elems;

    [rad] reverse gather while @n.any > 0 {
        (state $i = 0) %= RAD;
        take @n[$i] % RAD;
        @n[$i] div= RAD;
        $i++;
    }
}

sub rank(@n) { compress (compress(@n), @n - 1)}
sub unrank(Int $n) { my ($a, $b) = expand $n, 2; expand $a, $b + 1 }

my @list = (^10).roll((2..20).pick);
my $rank = rank @list;
say "[$@list] -> $rank -> [{unrank $rank}]";

for ^10 {
    my @unrank = unrank $_;
    say "$_ -> [$@unrank] -> {rank @unrank}";
}
```

#### Output:
```
[7 1 4 7 7 0 2 7 7 0 7 7] -> 20570633300796394530947471 -> [7 1 4 7 7 0 2 7 7 0 7 7]
0 -> [0] -> 0
1 -> [1] -> 1
2 -> [0 0] -> 2
3 -> [1 0] -> 3
4 -> [2] -> 4
5 -> [3] -> 5
6 -> [0 1] -> 6
7 -> [1 1] -> 7
8 -> [0 0 0] -> 8
9 -> [1 0 0] -> 9
```
