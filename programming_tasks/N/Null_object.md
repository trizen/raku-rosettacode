[1]: https://rosettacode.org/wiki/Null_object

# [Null object][1]





In Raku you can name the concept of `Nil`, but it not considered an object, but rather the *absence* of an object, more of a "bottom" type.  The closest analog in real objects is an empty list, but an empty list is considered defined, while `Nil.defined` always returns false.  `Nil` is what you get if you try to read off the end of a list, and `()` is just very easy to read off the end of...  `:-)`



If you try to put `Nil` into a container, you don't end up with a container that has `Nil` in it.  Instead the container reverts to an uninitialized state that is consistent with the declared type.  Hence, Raku has the notion of typed undefined values, that are real objects in the sense of "being there", but are generic in the sense of representing type information without being instantiated as a real object.  We call these *type objects* since they can stand in for real objects when one reasons about the types of objects.  So type objects fit into the type hierarchy just as normal objects do.  In physics terms, think of them as "type charge carriers" that are there for bookkeeping between the "real" particles.



All type objects derive from `Mu`, the most-undefined type
object, and the object most like "null" in many languages.  All other object types derive from `Mu`,
so it is like `Object` in other languages as well, except `Mu` also encompasses various objects that are not discrete, such as junctions.  So Raku distinguishes `Mu` from `Any`, which is the type that functions the most like a discrete, mundane object.



Mostly the user doesn't have to think about it.  All object containers behave like "Maybe" types in Haskell terms; they may either hold a valid value or a "nothing" of an appropriate type.
Most containers default to an object of type `Any` so you don't accidentally send quantum superpositions (junctions) around in your program.

```perl
my $var;
say $var.WHAT;      # Any()
$var = 42;
say $var.WHAT;      # Int()
say $var.defined;   # True
$var = Nil;
say $var.WHAT;      # Any()
say $var.defined    # False
```


You can declare a variable of type `Mu` if you wish to propagate superpositional types:

```perl
my Mu $junction;
say $junction.WHAT;      # Mu()
$junction = 1 | 2 | 3;
say $junction.WHAT;      # Junction()
```


Or you can declare a more restricted type than `Any`

```perl
my Str $str;
say $str.WHAT;      # Str()
$str = "I am a string.";
say $str.WHAT;      # Str()
$str = 42;          # (fails)
```


But in the Raku view of reality, it's completely bogus to ask
for a way "to see if an object is equivalent to the null object."
The whole point of such a non-object object is that it doesn't exist,
and can't participate in computations.  If you think you mean the
null object in Raku, you really mean some kind of generic object
that is uninstantiated, and hence undefined.  One of those is your "null object",
except there are many of them, so you can't just check for equivalence.  Use the `defined` predicate (or match on a subclass of your type that forces recognition of abstraction or definedness).



Raku also has `Failure` objects that, in addition to being
undefined carriers of type, are also carriers of the *reason*
for the value's undefinedness.  We tend view them as lazily thrown
exceptions, at least until you try to use them as defined values,
in which case they're thrown for real.
