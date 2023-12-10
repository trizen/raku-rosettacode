[1]: https://rosettacode.org/wiki/24_game/Solve

# [24 game/Solve][1]





### With EVAL



A loose translation of the Perl entry. Does not return every possible permutation of the possible solutions. Filters out duplicates (from repeated digits) and only reports the solution for a particular order of digits and operators with the fewest parenthesis (avoids reporting duplicate solutions only differing by unnecessary parenthesis). Does not guarantee the order in which results are returned.



Since Raku uses Rational numbers for division (whenever possible) there is no loss of precision as is common with floating point division. So a comparison like  (1 + 7) / (1 / 3) == 24 "Just Works"<sup>&#8482;</sup>

```perl
use MONKEY-SEE-NO-EVAL;

my @digits;
my $amount = 4;

# Get $amount digits from the user,
# ask for more if they don't supply enough
while @digits.elems < $amount {
    @digits.append: (prompt "Enter {$amount - @digits} digits from 1 to 9, "
    ~ '(repeats allowed): ').comb(/<[1..9]>/);
}
# Throw away any extras
@digits = @digits[^$amount];

# Generate combinations of operators
my @ops = [X,] <+ - * /> xx 3;

# Enough sprintf formats to cover most precedence orderings
my @formats = (
    '%d %s %d %s %d %s %d',
    '(%d %s %d) %s %d %s %d',
    '(%d %s %d %s %d) %s %d',
    '((%d %s %d) %s %d) %s %d',
    '(%d %s %d) %s (%d %s %d)',
    '%d %s (%d %s %d %s %d)',
    '%d %s (%d %s (%d %s %d))',
);

# Brute force test the different permutations
@digits.permutations».join.unique».comb.race.map: -> @p {
    for @ops -> @o {
        for @formats -> $format {
            my $result = .EVAL given my $string = sprintf $format, roundrobin(@p, @o, :slip);
            say "$string = 24" and last if $result and $result == 24;
        }
    }
}
```

#### Output:
```
Enter 4 digits from 1 to 9, (repeats allowed): 3711
(1 + 7) * 3 * 1 = 24
(1 + 7) * 3 / 1 = 24
(1 * 3) * (1 + 7) = 24
3 * (1 + 1 * 7) = 24
(3 * 1) * (1 + 7) = 24
3 * (1 / 1 + 7) = 24
(3 / 1) * (1 + 7) = 24
3 / (1 / (1 + 7)) = 24
(1 + 7) * 1 * 3 = 24
(1 + 7) / 1 * 3 = 24
(1 + 7) / (1 / 3) = 24
(1 * 7 + 1) * 3 = 24
(7 + 1) * 3 * 1 = 24
(7 + 1) * 3 / 1 = 24
(7 - 1) * (3 + 1) = 24
(1 + 1 * 7) * 3 = 24
(1 * 1 + 7) * 3 = 24
(1 / 1 + 7) * 3 = 24
(3 + 1) * (7 - 1) = 24
3 * (1 + 7 * 1) = 24
3 * (1 + 7 / 1) = 24
(3 * 1) * (7 + 1) = 24
(3 / 1) * (7 + 1) = 24
3 / (1 / (7 + 1)) = 24
(1 + 3) * (7 - 1) = 24
(1 * 3) * (7 + 1) = 24
(7 + 1) * 1 * 3 = 24
(7 + 1) / 1 * 3 = 24
(7 + 1) / (1 / 3) = 24
(7 - 1) * (1 + 3) = 24
(7 * 1 + 1) * 3 = 24
(7 / 1 + 1) * 3 = 24
3 * (7 + 1 * 1) = 24
3 * (7 + 1 / 1) = 24
3 * (7 * 1 + 1) = 24
3 * (7 / 1 + 1) = 24

Enter 4 digits from 1 to 9, (repeats allowed):  5 5 5 5
5 * 5 - 5 / 5 = 24

Enter 4 digits from 1 to 9, (repeats allowed): 8833
8 / (3 - 8 / 3) = 24
```


### No EVAL



Alternately, a version that doesn't use EVAL. More general case. Able to handle 3 or 4 integers, able to select the goal value.

```perl
my %*SUB-MAIN-OPTS = :named-anywhere;

sub MAIN (*@parameters, Int :$goal = 24) {
    my @numbers;
    if +@parameters == 1 {
        @numbers = @parameters[0].comb(/\d/);
        USAGE() and exit unless 2 < @numbers < 5;
    } elsif +@parameters > 4 {
        USAGE() and exit;
    } elsif +@parameters == 3|4 {
        @numbers = @parameters;
        USAGE() and exit if @numbers.any ~~ /<-[-\d]>/;
    } else {
        USAGE();
        exit if +@parameters == 2;
        @numbers = 3,3,8,8;
        say 'Running demonstration with: ', |@numbers, "\n";
    }
    solve @numbers, $goal
}

sub solve (@numbers, $goal = 24) {
    my @operators = < + - * / >;
    my @ops   = [X] @operators xx (@numbers - 1);
    my @perms = @numbers.permutations.unique( :with(&[eqv]) );
    my @order = (^(@numbers - 1)).permutations;
    my @sol;
    @sol[250]; # preallocate some stack space

    my $batch = ceiling +@perms/4;

    my atomicint $i;
    @perms.race(:batch($batch)).map: -> @p {
        for @ops -> @o {
            for @order -> @r {
                my $result = evaluate(@p, @o, @r);
                @sol[$i⚛++] = $result[1] if $result[0] and $result[0] == $goal;
            }
        }
    }
    @sol.=unique;
    say @sol.join: "\n";
    my $pl = +@sol == 1 ?? '' !! 's';
    my $sg = $pl ?? '' !! 's';
    say +@sol, " equation{$pl} evaluate{$sg} to $goal using: {@numbers}";
}

sub evaluate ( @digit, @ops, @orders ) {
    my @result = @digit.map: { [ $_, $_ ] };
    my @offset = 0 xx +@orders;

    for ^@orders {
        my $this  = @orders[$_];
        my $order = $this - @offset[$this];
        my $op    = @ops[$this];
        my $result = op( $op, @result[$order;0], @result[$order+1;0] );
        return [ NaN, Str ] unless defined $result;
        my $string = "({@result[$order;1]} $op {@result[$order+1;1]})";
        @result.splice: $order, 2, [ $[ $result, $string ] ];
        @offset[$_]++ if $order < $_ for ^@offset;
    }
    @result[0];
}

multi op ( '+', $m, $n ) { $m + $n }
multi op ( '-', $m, $n ) { $m - $n }
multi op ( '/', $m, $n ) { $n == 0 ?? fail() !! $m / $n }
multi op ( '*', $m, $n ) { $m * $n }

my $txt = "\e[0;96m";
my $cmd = "\e[0;92m> {$*EXECUTABLE-NAME} {$*PROGRAM-NAME}";
sub USAGE {
    say qq:to
    '========================================================================'
    {$txt}Supply 3 or 4 integers on the command line, and optionally a value
    to equate to.

    Integers may be all one group: {$cmd} 2233{$txt}
          Or, separated by spaces: {$cmd} 2 4 6 7{$txt}

    If you wish to supply multi-digit or negative numbers, you must
        separate them with spaces: {$cmd} -2 6 12{$txt}

    If you wish to use a different equate value,
    supply a new --goal parameter: {$cmd} --goal=17 2 -3 1 9{$txt}

    If you don't supply any parameters, will use 24 as the goal, will run a
    demo and will show this message.\e[0m
    ========================================================================
}
```


When supplied 1399 on the command line:


```
(((9 - 1) / 3) * 9)
((9 - 1) / (3 / 9))
((9 / 3) * (9 - 1))
(9 / (3 / (9 - 1)))
((9 * (9 - 1)) / 3)
(9 * ((9 - 1) / 3))
(((9 - 1) * 9) / 3)
((9 - 1) * (9 / 3))
8 equations evaluate to 24 using: 1 3 9 9
```
