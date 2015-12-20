[1]: http://rosettacode.org/wiki/24_game/Solve

# [24 game/Solve][1]

A loose translation of the Perl entry. Does not return every possible permutation of the possible solutions. Filters out duplicates (from repeated digits) and only reports the solution for a particular order of digits and operators with the fewest parenthesis (avoids reporting duplicate solutions only differing by unnecessary parenthesis).



Since Perl 6 uses Rational numbers for division (whenever possible) there is no loss of precision as is common with floating point division. So a comparison like  (1 + 7) / (1 / 3) == 24 "Just Works"<sup>&#8482;</sup>

```perl6
my @digits;
my $amount = 4;
 
# Get $amount digits from the user,
# ask for more if they don't supply enough
while @digits.elems < $amount {
    @digits ,= (prompt "Enter {$amount - @digits} digits from 1 to 9, "
    ~ '(repeats allowed): ').comb(/<[1..9]>/);
}
# Throw away any extras
@digits = @digits[^$amount];
 
# Generate combinations of operators
my @op = <+ - * />;
my @ops = map {my $a = $_; map {my $b = $_; map {[$a,$b,$_]}, @op}, @op}, @op;
 
# Enough sprintf formats to cover most precedence orderings
my @formats = (
    '%d %s %d %s %d %s %d',
    '(%d %s %d) %s %d %s %d',
    '(%d %s %d %s %d) %s %d',
    '((%d %s %d) %s %d) %s %d',
    '(%d %s %d) %s (%d %s %d)',
    '%d %s (%d %s %d %s %d)',
    '%d %s (%d %s (%d %s %d))',
);
 
# Brute force test the different permutations
for unique permutations @digits -> @p {
    for @ops -> @o {
        for @formats -> $format {
            my $string = sprintf $format, @p[0], @o[0],
                     @p[1], @o[1], @p[2], @o[2], @p[3];
            my $result = try { EVAL($string) };
            say "$string = 24" and last if $result and $result == 24;
        }
    }
}
 
# Perl 6 translation of Fischer-Krause ordered permutation algorithm
sub permutations (@array) {
    my @index = ^@array;
    my $last = @index[*-1];
    my (@permutations, $rev, $fwd);
    loop {
        push @permutations, [@array[@index]];
        $rev = $last;
        --$rev while $rev and @index[$rev-1] > @index[$rev];
        return @permutations unless $rev;
        $fwd = $rev;
        push @index, @index.splice($rev).reverse;
	++$fwd while @index[$rev-1] > @index[$fwd];
	@index[$rev-1,$fwd] = @index[$fwd,$rev-1];
    }
}
 
# Only return unique sub-arrays
sub unique (@array) {
    my %h = map { $_.Str => $_ }, @array;
    %h.values;
}
 
```

#### Output:
```
Enter 4 digits from 1 to 9, (repeats allowed): 3711
3 * (7 + 1 * 1) = 24
3 * (7 + 1 / 1) = 24
3 * (7 * 1 + 1) = 24
3 * (7 / 1 + 1) = 24
(3 + 1) * (7 - 1) = 24
3 * (1 + 7 * 1) = 24
3 * (1 + 7 / 1) = 24
(3 * 1) * (7 + 1) = 24
(3 / 1) * (7 + 1) = 24
3 / (1 / (7 + 1)) = 24
3 * (1 + 1 * 7) = 24
(3 * 1) * (1 + 7) = 24
3 * (1 / 1 + 7) = 24
(3 / 1) * (1 + 7) = 24
3 / (1 / (1 + 7)) = 24
(7 + 1) * 3 * 1 = 24
(7 + 1) * 3 / 1 = 24
(7 - 1) * (3 + 1) = 24
(7 + 1) * 1 * 3 = 24
(7 + 1) / 1 * 3 = 24
(7 + 1) / (1 / 3) = 24
(7 - 1) * (1 + 3) = 24
(7 * 1 + 1) * 3 = 24
(7 / 1 + 1) * 3 = 24
(1 + 3) * (7 - 1) = 24
(1 * 3) * (7 + 1) = 24
(1 * 3) * (1 + 7) = 24
(1 + 7) * 3 * 1 = 24
(1 + 7) * 3 / 1 = 24
(1 + 7) * 1 * 3 = 24
(1 + 7) / 1 * 3 = 24
(1 + 7) / (1 / 3) = 24
(1 * 7 + 1) * 3 = 24
(1 + 1 * 7) * 3 = 24
(1 * 1 + 7) * 3 = 24
(1 / 1 + 7) * 3 = 24

Enter 4 digits from 1 to 9, (repeats allowed):  5 5 5 5
5 * 5 - 5 / 5 = 24

Enter 4 digits from 1 to 9, (repeats allowed): 8833
8 / (3 - 8 / 3) = 24
```