[1]: https://rosettacode.org/wiki/Latin_Squares_in_reduced_form

# [Latin Squares in reduced form][1]



```perl
# utilities: factorial, sub-factorial, derangements
sub  postfix:<!>($n) { (constant f = 1, |[\×] 1..*)[$n] }
sub   prefix:<!>($n) { (1, 0, 1, -> $a, $b { ($++ + 2) × ($b + $a) } ... *)[$n] }
sub derangements(@l) { @l.permutations.grep(-> @p { none(@p Zeqv @l) }) }

sub LS-reduced (Int $n) {
    return [1] if $n == 1;

    my @LS;
    my @l = 1 X+ ^$n;
    my %D = derangements(@l).classify(*.[0]);

    for [X] (^(!$n/($n-1))) xx $n-1 -> $tuple {
        my @d.push: @l;
        @d.push: %D{2}[$tuple[0]];
        LOOP:
        for 3 .. $n -> $x {
            my @try = |%D{$x}[$tuple[$x-2]];
            last LOOP if any @try »==« @d[$_] for 1..@d-1;
            @d.push: @try;
        }
        next unless @d == $n and [==] [Z+] @d;
        @LS.push: @d;
    }
    @LS
}

say .join("\n") ~ "\n" for LS-reduced(4);
for 1..6 -> $n {
    printf "Order $n: Size %-4d x $n! x {$n-1}! => Total %d\n", $_, $_ * $n! * ($n-1)! given LS-reduced($n).elems
}
```

#### Output:
```
1 2 3 4
2 1 4 3
3 4 1 2
4 3 2 1

1 2 3 4
2 1 4 3
3 4 2 1
4 3 1 2

1 2 3 4
2 3 4 1
3 4 1 2
4 1 2 3

1 2 3 4
2 4 1 3
3 1 4 2
4 3 2 1

Order 1: Size 1    x 1! x 0! => Total 1
Order 2: Size 1    x 2! x 1! => Total 2
Order 3: Size 1    x 3! x 2! => Total 12
Order 4: Size 4    x 4! x 3! => Total 576
Order 5: Size 56   x 5! x 4! => Total 161280
Order 6: Size 9408 x 6! x 5! => Total 812851200
```
