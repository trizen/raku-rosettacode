[1]: https://rosettacode.org/wiki/Calkin-Wilf_sequence

# [Calkin-Wilf sequence][1]

In Raku, arrays are indexed from 0. The Calkin-Wilf sequence does not have a term defined at 0.



This implementation includes a bogus undefined value at position 0, having the bogus first term shifts the indices up by one, making the ordinal position and index match. Useful due to how reversibility function works.

```perl
my @calkin-wilf = Any, 1, {1 / (.Int × 2 + 1 - $_)} … *;
 
# Rational to Calkin-Wilf index
sub r2cw (Rat $rat) { :2( join '', flat (flat (1,0) xx *) Zxx reverse r2cf $rat ) }
 
# The task
 
say "First twenty terms of the Calkin-Wilf sequence: ",
    @calkin-wilf[1..20]».&prat.join: ', ';
 
say "\n99991st through 100000th: ",
    (my @tests = @calkin-wilf[99_991 .. 100_000])».&prat.join: ', ';
 
say "\nCheck reversibility: ", @tests».Rat».&r2cw.join: ', ';
 
say "\n83116/51639 is at index: ", r2cw 83116/51639;
 
 
# Helper subs
sub r2cf (Rat $rat is copy) { # Rational to continued fraction
    gather loop {
	    $rat -= take $rat.floor;
	    last if !$rat;
	    $rat = 1 / $rat;
    }
}
 
sub prat ($num) { # pretty Rat
    return $num unless $num ~~ Rat|FatRat;
    return $num.numerator if $num.denominator == 1;
    $num.nude.join: '/';
}
```

#### Output:
```
First twenty terms of the Calkin-Wilf sequence: 1, 1/2, 2, 1/3, 3/2, 2/3, 3, 1/4, 4/3, 3/5, 5/2, 2/5, 5/3, 3/4, 4, 1/5, 5/4, 4/7, 7/3, 3/8

99991st through 100000th: 1085/303, 303/1036, 1036/733, 733/1163, 1163/430, 430/987, 987/557, 557/684, 684/127, 127/713

Check reversibility: 99991, 99992, 99993, 99994, 99995, 99996, 99997, 99998, 99999, 100000

83116/51639 is at index: 123456789
```
