[1]: http://rosettacode.org/wiki/Pick_random_element

# [Pick random element][1]

In a nutshell, picking an element from a list
is implemented with a method conveniently called "pick":

```perl
say (1, 2, 3).pick;
```


There are various ways of doing something similar, though.
Perl 6 has actually two methods (with associated functional forms)
to return random elements depending on whether you are doing selection
with or without replacement.



Selection with replacement: (roll of a die)

```perl
(1..6).roll;    # return 1 random value in the range 1 through 6
(1..6).roll(3); # return a list of 3 random values in the range 1 through 6
(1..6).roll(*); # return a lazy infinite list of random values in the range 1 through 6
```


Selection without replacement: (pick a card from a deck)

```perl
# define the deck
my @deck = <2 3 4 5 6 7 8 9 J Q K A> X~ <♠ ♣ ♥ ♦>;
@deck.pick;    # Pick a card
@deck.pick(5); # Draw 5
@deck.pick(*); # Get a shuffled deck
```


Or you can always use the normal <tt>rand</tt> built-in
to generate a subscript (which automatically truncates any fractional part):

```perl
@array[@array * rand]
```


However, the <tt>pick</tt> and <tt>roll</tt> methods (not to be confused
with the pick-and-roll method in basketball) are more general
insofar as they may be used on any enumerable type:

```perl
say Bool.pick;  # returns either True or False
```