[1]: https://rosettacode.org/wiki/Pick_random_element

# [Pick random element][1]

In a nutshell, picking an element from a list
is implemented with a method conveniently called "pick":

```raku
say (1, 2, 3).pick;
```


There are various ways of doing something similar, though.
Perl 6 has actually two methods (with associated functional forms)
to return random elements depending on whether you are doing selection
with or without replacement.



Selection with replacement: (roll of a die)

```raku
say (1..6).roll;          # return 1 random value in the range 1 through 6
say (1..6).roll(3);       # return a list of 3 random values in the range 1 through 6
say (1..6).roll(*)[^100]; # return first 100 values from a lazy infinite list of random values in the range 1 through 6
```


Selection without replacement: (pick a card from a deck)

```raku
# define the deck
my @deck = <2 3 4 5 6 7 8 9 J Q K A> X~ <♠ ♣ ♥ ♦>;
say @deck.pick;    # Pick a card
say @deck.pick(5); # Draw 5
say @deck.pick(*); # Get a shuffled deck
```


Or you can always use the normal `rand` built-in
to generate a subscript (which automatically truncates any fractional part):

```raku
@array[@array * rand]
```


However, the `pick` and `roll` methods (not to be confused
with the pick-and-roll method in basketball) are more general
insofar as they may be used on any enumerable type:

```raku
say Bool.pick;  # returns either True or False
```