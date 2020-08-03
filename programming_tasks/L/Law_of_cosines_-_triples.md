[1]: https://rosettacode.org/wiki/Law_of_cosines_-_triples

# [Law of cosines - triples][1]

```perl
multi triples (60, $n) {
    my %sq = (1..$n).map: { .² => $_ };
    my %triples;
    (1..^$n).race(:8degree).map: -> $a {
        for $a^..$n -> $b {
            my $cos = $a * $a + $b * $b - $a * $b;
            %triples{~($a, %sq{$cos}, $b)}++ and last if %sq{$cos}:exists;
        }
    }
    %triples.keys
}
 
multi triples (90, $n) {
    my %sq = (1..$n).map: { .² => $_ };
    my %triples;
    (1..^$n).race(:8degree).map: -> $a {
        for $a^..$n -> $b {
            my $cos = $a * $a + $b * $b;
            %triples{~($a, $b, %sq{$cos})}++ and last if %sq{$cos}:exists;
        }
    }
    %triples.keys
}
 
multi triples (120, $n) {
    my %sq = (1..$n).map: { .² => $_ };
    my %triples;
    (1..^$n).race(:8degree).map: -> $a {
        for $a^..$n -> $b {
            my $cos = $a * $a + $b * $b + $a * $b;
           %triples{~($a, $b, %sq{$cos})}++ and last if %sq{$cos}:exists;
        }
    }
    %triples.keys
}
 
use Sort::Naturally;
 
my $n = 13;
say "Integer triangular triples for sides 1..$n:";
for 120, 90, 60 -> $angle {
    my @itt = triples($angle, $n);
    if $angle == 60 { push @itt, "$_ $_ $_" for 1..$n }
    printf "Angle %3d° has %2d solutions: %s\n", $angle, +@itt, @itt.sort(*.&naturally).join(', ');
}
 
my ($angle, $count) = 60, 10_000;
say "\nExtra credit:";
say "$angle° integer triples in the range 1..$count where the sides are not all the same length: ", +triples($angle, $count);
```

#### Output:
```
Integer triangular triples for sides 1..13:
Angle 120° has  2 solutions: 3 5 7, 7 8 13
Angle  90° has  3 solutions: 3 4 5, 5 12 13, 6 8 10
Angle  60° has 15 solutions: 1 1 1, 2 2 2, 3 3 3, 3 7 8, 4 4 4, 5 5 5, 5 7 8, 6 6 6, 7 7 7, 8 8 8, 9 9 9, 10 10 10, 11 11 11, 12 12 12, 13 13 13

Extra credit:
60° integer triples in the range 1..10000 where the sides are not all the same length: 18394
```