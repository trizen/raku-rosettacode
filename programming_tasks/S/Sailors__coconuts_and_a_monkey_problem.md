[1]: http://rosettacode.org/wiki/Sailors,_coconuts_and_a_monkey_problem

# [Sailors, coconuts and a monkey problem][1]

There is nowhere in the spec where it explicitly states that the sailors cannot equally share zero coconuts in the morning. Actually, The On-Line Encyclopedia of Integer Sequences [A002021](http://oeis.org/A002021) considers the cases for 1 and 2 sailors equally sharing zero coconuts in the morning to be the correct answer.



This will test combinations of sailors and coconuts to see if they form a valid pairing. The first 6 are done using brute force, testing every combination until a valid one is found. For cases of 7 to 15 sailors, it uses a carefully crafted filter to drastically reduce the number of trials needed to find a valid case (to one, as it happens...&#160;:-) )

```perl6
my @ones = flat 'th', 'st', 'nd', 'rd', 'th' xx 6;
my @teens = 'th' xx 10;
my @suffix = lazy flat (@ones, @teens, @ones xx 8) xx *;
 
# brute force the first six
for 1 .. 6 -> $sailors { for $sailors .. * -> $coconuts { last if check( $sailors, $coconuts ) } }
 
# finesse 7 through 15
for 7 .. 15 -> $sailors { next if check( $sailors, coconuts( $sailors ) ) }
 
sub is_valid ( $sailors is copy, $nuts is copy ) {
    return 0, 0 if $sailors == $nuts == 1;
    my @shares;
    for ^$sailors {
        return () unless $nuts % $sailors == 1;
        push @shares, ($nuts - 1) div $sailors;
        $nuts -= (1 + $nuts div $sailors);
    }
    push @shares, $nuts div $sailors;
    return @shares if !?($nuts % $sailors);
}
 
sub check ($sailors, $coconuts) {
    if my @piles = is_valid($sailors, $coconuts) {
        say "\nSailors $sailors: Coconuts $coconuts:";
        for ^(@piles - 1) -> $k {
             say "{$k+1}@suffix[$k+1] takes @piles[$k], gives 1 to the monkey."
        }
        say "The next morning, each sailor takes @piles[*-1]\nwith none left over for the monkey.";
        return True;
    }
    False;
}
 
multi sub coconuts ( $sailors where { $sailors % 2 == 0 } ) { ($sailors - 1) * ($sailors ** $sailors - 1) }
multi sub coconuts ( $sailors where { $sailors % 2 == 1 } ) { $sailors ** $sailors - $sailors + 1 }
```

#### Output:
```
Sailors 1: Coconuts 1:
1st takes 0, gives 1 to the monkey.
The next morning, each sailor takes 0
with none left over for the monkey.

Sailors 2: Coconuts 3:
...

Sailors 5: Coconuts 3121:
1st takes 624, gives 1 to the monkey.
2nd takes 499, gives 1 to the monkey.
3rd takes 399, gives 1 to the monkey.
4th takes 319, gives 1 to the monkey.
5th takes 255, gives 1 to the monkey.
The next morning, each sailor takes 204
with none left over for the monkey.

Sailors 6: Coconuts 233275:
1st takes 38879, gives 1 to the monkey.
2nd takes 32399, gives 1 to the monkey.
3rd takes 26999, gives 1 to the monkey.
4th takes 22499, gives 1 to the monkey.
5th takes 18749, gives 1 to the monkey.
6th takes 15624, gives 1 to the monkey.
The next morning, each sailor takes 13020
with none left over for the monkey.

Sailors 7: Coconuts 823537:
...

Sailors 15: Coconuts 437893890380859361:
...
```