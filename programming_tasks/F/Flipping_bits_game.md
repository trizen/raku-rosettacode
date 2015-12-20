[1]: http://rosettacode.org/wiki/Flipping_bits_game

# [Flipping bits game][1]

Pass in a parameter to set the square size for the puzzle. (Defaults to 4.) Arbitrarily limited to between 1 and 26. Yes, you can choose to solve a 1 element square puzzle, but it won't be very challenging. Accepts upper or lower case letters for columns. Disregards any out-of-range indices. Enter a blank or 0 (zero) to exit.

```perl
sub MAIN ($square = 4) {
    say "{$square}? Seriously?" and exit if $square < 1 or $square > 26;
    my %bits = map { $_ => %( map { $_ => 0 }, ('A' .. *)[^ $square] ) },
        (1 .. *)[^ $square];
    scramble %bits;
    my $target = build %bits;
    scramble %bits until build(%bits) ne $target;
    display($target, %bits);
    my $turns = 0;
    while my $flip = prompt "Turn {++$turns}: Flip which row / column? " {
        flip $flip.match(/\w/).uc, %bits;
        if display($target, %bits) {
            say "Hurray! You solved it in $turns turns.";
            last;
        }
    }
}
 
sub display($goal, %hash) {
    shell('clear');
    say "Goal\n$goal\nYou";
    my $this = build %hash;
    say $this;
    return ($goal eq $this);
}
 
sub flip ($a, %hash) {
    given $a {
        when any(keys %hash) {
            %hash{$a}{$_} = %hash{$a}{$_} +^ 1 for %hash{$a}.keys
        };
        when any(keys %hash{'1'}) {
            %hash{$_}{$a} = %hash{$_}{$a} +^ 1 for %hash.keys
        };
    }
}
 
sub build (%hash) {
    my $string = '   ';
    $string ~= sprintf "%2s ", $_ for sort keys %hash{'1'};
    $string ~= "\n";
    for sort keys %hash -> $key {
        $string ~= sprintf "%2s ", $key;
        $string ~= sprintf "%2s ", %hash{$key}{$_} for sort keys %hash{$key};
        $string ~=  "\n";
    };
    $string
}
 
sub scramble(%hash) {
    my @keys = keys %hash;
    @keys ,= keys %hash{'1'};
    flip $_,  %hash for @keys.pick( @keys/2 );
}
```


A sample 3 x 3 game might look like this:


#### Output:
```
Goal
    A  B  C 
 1  1  1  0 
 2  0  0  1 
 3  1  1  0 

You
    A  B  C 
 1  0  0  0 
 2  1  1  1 
 3  1  1  1 

Turn 1: Flip which row / column? 2

Goal
    A  B  C 
 1  1  1  0 
 2  0  0  1 
 3  1  1  0 

You
    A  B  C 
 1  0  0  0 
 2  0  0  0 
 3  1  1  1 

Turn 2: Flip which row / column? 1

Goal
    A  B  C 
 1  1  1  0 
 2  0  0  1 
 3  1  1  0 

You
    A  B  C 
 1  1  1  1 
 2  0  0  0 
 3  1  1  1 

Turn 3: Flip which row / column? c

Goal
    A  B  C 
 1  1  1  0 
 2  0  0  1 
 3  1  1  0 

You
    A  B  C 
 1  1  1  0 
 2  0  0  1 
 3  1  1  0 

Hurray! You solved it in 3 turns.
```