[1]: http://rosettacode.org/wiki/Null_object

# [Null object][1]

In Perl 6 you can name the concept of <tt>Nil</tt>, but it not considered an object, but rather the _absence_ of an object, more of a "bottom" type. The closest analog in real objects is an empty list, but an empty list is considered defined, while <tt>Nil.defined</tt> always returns false. <tt>Nil</tt> is what you get if you try to read off the end of a list, and <tt>()</tt> is just very easy to read off the end of... <tt>:-)</tt>



If you try to put <tt>Nil</tt> into a container, you don't end up with a container that has <tt>Nil</tt> in it. Instead the container reverts to an uninitialized state that is consistent with the declared type. Hence, Perl 6 has the notion of typed undefined values, that are real objects in the sense of "being there", but are generic in the sense of representing type information without being instantiated as a real object. We call these _type objects_ since they can stand in for real objects when one reasons about the types of objects. So type objects fit into the type hierarchy just as normal objects do. In physics terms, think of them as "type charge carriers" that are there for bookkeeping between the "real" particles.



All type objects derive from <tt>Mu</tt>, the most-undefined type
object, and the object most like "null" in many languages. All other object types derive from <tt>Mu</tt>,
so it is like <tt>Object</tt> in other languages as well, except <tt>Mu</tt> also encompasses various objects that are not discrete, such as junctions. So Perl 6 distinguishes <tt>Mu</tt> from <tt>Any</tt>, which is the type that functions the most like a discrete, mundane object.



Mostly the user doesn't have to think about it. All object containers behave like "Maybe" types in Haskell terms; they may either hold a valid value or a "nothing" of an appropriate type.
Most containers default to an object of type <tt>Any</tt> so you don't accidentally send quantum superpositions (junctions) around in your program.

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


You can declare a variable of type <tt>Mu</tt> if you wish to propagate superpositional types:

```perl
my Mu $junction;
say $junction.WHAT;      # Mu()
$junction = 1 | 2 | 3;
say $junction.WHAT;      # Junction()
```


Or you can declare a more restricted type than <tt>Any</tt>

```perl
my Str $str;
say $str.WHAT;      # Str()
$str = "I am a string.";
say $str.WHAT;      # Str()
$str = 42;          # (fails)
```


But in the Perl 6 view of reality, it's completely bogus to ask
for a way "to see if an object is equivalent to the null object."
The whole point of such a non-object object is that it doesn't exist,
and can't participate in computations. If you think you mean the
null object in Perl 6, you really mean some kind of generic object
that is uninstantiated, and hence undefined. One of those is your "null object",
except there are many of them, so you can't just check for equivalence. Use the <tt>defined</tt> predicate (or match on a subclass of your type that forces recognition of abstraction or definedness).



Perl 6 also has <tt>Failure</tt> objects that, in addition to being
undefined carriers of type, are also carriers of the _reason_
for the value's undefinedness. We tend view them as lazily thrown
exceptions, at least until you try to use them as defined values,
in which case they're thrown for real.