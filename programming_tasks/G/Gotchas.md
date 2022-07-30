[1]: https://rosettacode.org/wiki/Gotchas

# [Gotchas][1]

Raku embraces the philosophy of [DWIM](https://en.wikipedia.org/wiki/DWIM) or "Do What I Mean". A DWIMmy
language tends to avoid lots of boilerplate code, and accept a certain amount of
imprecision to return the result that, for the vast majority of the time, is
what the user intended.



For example, a numeric string may be treated as either a numeric value or a
string literal, without needing to explicitly coerce it to one or the other.
This makes for easy translation of ideas into code, and is generally a good
thing. HOWEVER, it doesn't always work out. Sometimes it leads to unexpected
behavior, commonly referred to as [WAT](https://www.destroyallsoftware.com/talks/wat).



It is something of a running joke in the Raku community that "For every DWIM,
there is an equal and opposite WAT".



[Larry Wall](https://en.wikipedia.org/wiki/Larry_Wall), the author and designer of [Perl](https://rosettacode.org/wiki/Category:Perl) and
lead designer of [Raku](https://rosettacode.org/wiki/Category:Raku), coined a term to describe this DWIM / WAT
continuum. [The Waterbed Theory of Computational Complexity](https://en.wikipedia.org/wiki/Waterbed_theory).



The Waterbed theory is the observation that complicated systems contain a minimum
amount of complexity, and that attempting to "push down" the complexity of such
a system in one place will invariably cause complexity to "pop up" elsewhere.



Much like how in a waterbed mattress, it is possible to push down the mattress in one
place, but the displaced water will always cause the mattress to rise elsewhere,
because water does not compress. It is impossible to push down the waterbed
everywhere at once.



There is a whole chapter in the [Raku documentation](https://docs.raku.org) about
["Traps to Avoid"](https://docs.raku.org/language/traps) when beginning in Raku,
most of which, at least partially are due to WATs arising from DWIMmy behavior
someplace else.





Expanding on the numeric string example cited above; numeric values and numeric
strings may be used almost interchangeably in most cases.

```perl
say 123 ~ 456; # join two integers together
say "12" + "5.7"; # add two numeric strings together
say .sqrt for <4 6 8>; # take the square root of several allomorphic numerics
```

#### Output:
```
123456
17.7
2
2.449489742783178
2.8284271247461903
```


You can run into problems though with certain constructs that are more strict about
object typing.



A Bag is a "counting" construct. It takes a collection and counts how many of
each object are within. Works great for strings.

```perl
say my $bag = <a b a c a b d>.Bag;
say $bag{'a'}; # a count?
say $bag< a >; # another way
```

#### Output:
```
Bag(a(3) b(2) c d)
3
3
```


But numerics can present unobvious problems.

```perl
say my $bag = (1, '1', '1', <1 1 1>).Bag;
say $bag{ 1 }; # how many 1s?
say $bag{'1'}; # wait, how many?
say $bag< 1 >; # WAT
dd $bag; # The different numeric types LOOK the same, but are different types behind the scenes
```

#### Output:
```
Bag(1 1(2) 1(3))
1
2
3
Bag $bag = (1=>1,"1"=>2,IntStr.new(1, "1")=>3).Bag
```


The different '1's are distinctive to the type system even if they visually look
identical when printing them to the console. They all have a value of 1 but are
respectively and Int, a String, and an IntStr allomorph. Many of the "collective"
objects have this property (Bags, Sets, Maps, etc.) This behavior is correct
but can be very jarring when you are used to being able to use numeric strings
and numeric values nearly interchangeably.





Another such DWIMmy construct, which can trip up even experienced Raku programmers is
[The Single Argument Rule](https://docs.raku.org/language/list#index-entry-Single_Argument_Rule).



Single argument is a special exception to parameterization rules causing an iterable
object to be be automatically flattened when passed to an iterator if only a single
object is passed.





E.G. Say we have two small list objects; `(1,2,3)` and `(4,5,6)`,
and we want to print the contents to the console.

```perl
.say for (1,2,3), (4,5,6);
```

#### Output:
```
(1,2,3)
(4,5,6)
```


However, if we only pass a single list to the iterator (`for`), it will
flatten the object due to the single argument rule.

```perl
.say for (1,2,3);
```

#### Output:
```
1
2
3
```


If we want the multiple object non-flattening behavior, we need to "fake it out"
by adding a trailing comma to signal to the compiler that this should be treated
like multiple object parameters even if there is only one. (Note the trailing comma after the list.)

```perl
.say for (1,2,3),;
```

#### Output:
```
(1,2,3)
```


Conversely, if we want the flattening behavior when passing multiple objects, we need to manually, explicitly flatten the objects.

```perl
.say for flat (1,2,3), (4,5,6);
```

#### Output:
```
1
2
3
4
5
6
```


Single argument mostly arose in Raku to make it act more like Perl 5, for which
it was originally conceived of as a replacement. Perl 5 flattens collective
object parameters by default, and the original non-flattening behavior was
*extremely* confusing to early Perl / Raku crossover programmers. Larry Wall
came up with single argument to reduce confusion and increase DWIMiness, and it
has overall, but it still leads to the occasional WAT when programmers first bump up against it.
