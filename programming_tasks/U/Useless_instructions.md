[1]: https://rosettacode.org/wiki/Useless_instructions

# [Useless instructions][1]

I disagree with the whole premise. There are pretty darn few instructions that are completely useless in *any* language unless *you are using them in ways not intended*, or are *deliberately structuring them to be useless*; and most of the existing examples point that out.



.oO( This chainsaw is crap at driving screws in... ) Well duh.



.oO( I've multiplied this numeric value by 1 millions of times and it still hasn't changed! ) Thanks for your contribution, Captain Oblivious.



Redundant instructions / operations on the other hand... Syntactically different operations that *can* be used to achieve the same result abound in higher level languages. In general they are intended to be used in slightly different circumstances, or are just syntactic sugar to condense a multi-step operation down to a more compact representation. Raku is just **full** of syntactic sugar operations.



Programming languages aren't for computers, they are for people. If they were for computers, we would be programming in binary code. Having an expressive language that helps the programmer transform his thoughts into code leads to "redundancies" due to allowing for shades of meaning that make sense to people though may be insignificant to computers.



Some examples: Say you have a variable $x that holds an integer, and you want to add 1 to it.

```perl
my $x = 1;
 
$x = $x + 1;  # classic
$x += 1;      # classic abbreviated
++$x;         # pre-increment
$x++;         # post-increment
$x.=succ;     # method, find successor
```


*Raku is by no means unique in that regard.* Each one of those nominally accomplishes the same thing, though they go about it in slightly different ways, and are intended for slightly different purposes. Redundant on the face of it, but different reasons for each.



There may be different operations that essentially perform the same function that have been retained for historical compatibility and / or ease for crossover programmers. the `for next` construct really only exists in Raku for programmer convenience. Internally it is converted to a map operation. Programmers can use map directly, but if they come from a language that doesn't provide map, and aren't used to it, `for next` is a easy stepping stone.

```perl
for 1..10 { next if $_ %% 2; say '_' x $_ };  # for next
map { next if $_ %% 2; say '_' x $_ }, 1..10; # equivalent map
```


Syntactic sugar (typically) replaces some longer expression with a shorter, more easily typed one. Raku abounds with them.

```perl
 
my @a1 = Q:w[one two three]; # list of strings using the Q sublanguage
my @a2 = <one two three>;    # syntactic sugar for the same operation
 
say (1,2,3,4,5,6,7).reduce(&infix:<*>); # multiply all of the items in a list together 
say [*] (1,2,3,4,5,6,7);                # same operation, sweeter syntax
 
```
