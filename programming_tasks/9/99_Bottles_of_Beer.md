[1]: https://rosettacode.org/wiki/99_Bottles_of_Beer

# [99 Bottles of Beer][1]

### A Simple Way

```raku
my $b = 99;
 
repeat while --$b {
    say "{b $b} on the wall";
    say "{b $b}";
    say "Take one down, pass it around";
    say "{b $b-1} on the wall";
    say "";
}
 
sub b($b) {
    "$b bottle{'s' if $b != 1} of beer";
}
```


### A Clearer Way



Similar to "A Simple Way", but with proper variable and subroutine naming, declarator documentation, strongly-typed function definition, better code reuse, and external ternary logic.

```raku
for 99...1 -> $bottles {
    sing $bottles, :wall;
    sing $bottles;
    say  "Take one down, pass it around";
    sing $bottles - 1, :wall;
    say  "";
}
 
#| Prints a verse about a certain number of beers, possibly on a wall.
sub sing(
    Int $number, #= Number of bottles of beer.
    Bool :$wall, #= Mention that the beers are on a wall?
) {
    my $quantity = $number == 0 ?? "No more"      !! $number;
    my $plural   = $number == 1 ?? ""             !! "s";
    my $location = $wall        ?? " on the wall" !! "";
    say "$quantity bottle$plural of beer$location"
}
```


### A More Extravagant Way

```raku
my @quantities = flat (99 ... 1), 'No more', 99;
my @bottles = flat 'bottles' xx 98, 'bottle', 'bottles' xx 2;
my @actions = flat 'Take one down, pass it around' xx 99,
              'Go to the store, buy some more';
 
for @quantities Z @bottles Z @actions Z
    @quantities[1 .. *] Z @bottles[1 .. *]
    -> ($a, $b, $c, $d, $e) {
    say "$a $b of beer on the wall";
    say "$a $b of beer";
    say $c;
    say "$d $e of beer on the wall\n";
}
```