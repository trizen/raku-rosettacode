[1]: http://rosettacode.org/wiki/String_comparison

# [String comparison][1]

Perl 6 uses strong typing dynamically (and gradual typing statically), but normal string and numeric comparisons are coercive. (You may use generic comparison operators if you want polymorphic comparison—but usually you don't.&#160;:)



String comparisons never do case folding because that's a very complicated subject in the modern world of Unicode. (You can explicitly apply an appropriate case-folding function to the arguments before doing the comparison, or for "equality" testing you can do matching with a case-insensitive regex, assuming Unicode's language-neutral case-folding rules are okay.)

```perl
sub compare($a,$b) {
    my $A = "{$a.WHAT.^name} '$a'";
    my $B = "{$b.WHAT.^name} '$b'";
 
    if $a eq $b { say "$A and $B are lexically equal" }
    if $a ne $b { say "$A and $B are not lexically equal" }
 
    if $a gt $b { say "$A is lexically after $B" }
    if $a lt $b { say "$A is lexically before than $B" }
 
    if $a ge $b { say "$A is not lexically before $B" }
    if $a le $b { say "$A is not lexically after $B" }
 
    if $a === $b { say "$A and $B are identical objects" }
    if $a !=== $b { say "$A and $B are not identical objects" }
 
    if $a eqv $b { say "$A and $B are generically equal" }
    if $a !eqv $b { say "$A and $B are not generically equal" }
 
    if $a before $b { say "$A is generically after $B" }
    if $a after $b { say "$A is generically before $B" }
 
    if $a !after $b { say "$A is not generically before $B" }
    if $a !before $b { say "$A is not generically after $B" }
 
    say "The lexical relationship of $A and $B is { $a leg $b }" if $a ~~ Stringy;
    say "The generic relationship of $A and $B is { $a cmp $b }";
    say "The numeric relationship of $A and $B is { $a <=> $b }" if $a ~~ Numeric;
    say '';
}
 
compare 'YUP', 'YUP';
compare 'BALL', 'BELL';
compare 24, 123;
compare 5.1, 5;
compare 5.1e0, 5 + 1/10;
```

#### Output:
```
Str 'YUP' and Str 'YUP' are lexically equal
Str 'YUP' is not lexically before Str 'YUP'
Str 'YUP' is not lexically after Str 'YUP'
Str 'YUP' and Str 'YUP' are identical objects
Str 'YUP' and Str 'YUP' are generically equal
Str 'YUP' is not generically before Str 'YUP'
Str 'YUP' is not generically after Str 'YUP'
The lexical relationship of Str 'YUP' and Str 'YUP' is Same
The generic relationship of Str 'YUP' and Str 'YUP' is Same

Str 'BALL' and Str 'BELL' are not lexically equal
Str 'BALL' is lexically before than Str 'BELL'
Str 'BALL' is not lexically after Str 'BELL'
Str 'BALL' and Str 'BELL' are not identical objects
Str 'BALL' and Str 'BELL' are not generically equal
Str 'BALL' is generically after Str 'BELL'
Str 'BALL' is not generically before Str 'BELL'
The lexical relationship of Str 'BALL' and Str 'BELL' is Increase
The generic relationship of Str 'BALL' and Str 'BELL' is Increase

Int '24' and Int '123' are not lexically equal
Int '24' is lexically after Int '123'
Int '24' is not lexically before Int '123'
Int '24' and Int '123' are not identical objects
Int '24' and Int '123' are not generically equal
Int '24' is generically after Int '123'
Int '24' is not generically before Int '123'
The generic relationship of Int '24' and Int '123' is Increase
The numeric relationship of Int '24' and Int '123' is Increase

Rat '5.1' and Int '5' are not lexically equal
Rat '5.1' is lexically after Int '5'
Rat '5.1' is not lexically before Int '5'
Rat '5.1' and Int '5' are not identical objects
Rat '5.1' and Int '5' are not generically equal
Rat '5.1' is generically before Int '5'
Rat '5.1' is not generically after Int '5'
The generic relationship of Rat '5.1' and Int '5' is Decrease
The numeric relationship of Rat '5.1' and Int '5' is Decrease

Num '5.1' and Rat '5.1' are lexically equal
Num '5.1' is not lexically before Rat '5.1'
Num '5.1' is not lexically after Rat '5.1'
Num '5.1' and Rat '5.1' are not identical objects
Num '5.1' and Rat '5.1' are not generically equal
Num '5.1' is not generically before Rat '5.1'
Num '5.1' is not generically after Rat '5.1'
The generic relationship of Num '5.1' and Rat '5.1' is Same
The numeric relationship of Num '5.1' and Rat '5.1' is Same
```