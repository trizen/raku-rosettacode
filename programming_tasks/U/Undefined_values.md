[1]: https://rosettacode.org/wiki/Undefined_values

# [Undefined values][1]


Raku has "interesting" values of undef, but unlike Perl 5, doesn't actually have a value named `undef`.  Instead, several very different meanings of undefinedness are distinguished.  First, `Nil` represents the absence of a value.  The absence of a value cannot be stored.  Instead, an attempt to assign `Nil` to a storage location causes that location to revert to its uninitialized state, however that is defined.

```perl
my $x; $x = 42; $x = Nil; say $x.WHAT; # prints Any()
```


This `Any` is an example of another kind of undefined type, which is a typed undef.  All reference types have an undefined value representing the type.  You can think of it as a sort of "type gluon" that carries a type charge without being a "real" particle.  Hence there are undefined values whose names represent types, such as `Int`, `Num`, `Str`, and all the other object types in Raku.  As generic objects, such undefined values carry the same metaobject pointer that a real object of the type would have, without being instantiated as a real object.  As such, these types are in the type hierarchy.  For example, `Int` derives from `Cool`, `Cool` derives from `Any`, and `Any` derives from `Mu`, the most general undefined object (akin to Object in other languages).  Since they are real objects of the type, even if undefined, they can be used in reasoning about the type.  You don't have to instantiate a `Method` object in order to ask if `Method` is derived from `Routine`, for instance.

```perl
say Method ~~ Routine;  # Bool::True
```


Variables default to `Any`, unless declared to be of another type:

```perl
my     $x; say $x.WHAT; # Any()
my Int $y; say $y.WHAT; # Int()
my Str $z; say $z.WHAT; # Str()
```


The user-interface for definedness are [type smilies](http://design.raku.org/S12.html#Abstract_vs_Concrete_types) and the `with`-statement.

```perl
my Int:D $i = 1; # if $i has to be defined you must provide a default value
multi sub foo(Int:D $i where * != 0){ (0..100).roll / $i } # we will never divide by 0
multi sub foo(Int:U $i){ die 'WELP! $i is undefined' } # because undefinedness is deadly

with $i { say 'defined' } # as "if" is looking for Bool::True, "with" is looking for *.defined
with 0 { say '0 may not divide but it is defined' }
```


There are further some [operators](http://design.raku.org/S03.html) for your convenience.

```perl
my $is-defined = 1;
my $ain't-defined = Any;
my $doesn't-matter;
my Any:D $will-be-defined = $ain't-defined // $is-defined // $doesn't-matter;

my @a-mixed-list = Any, 1, Any, 'a';
$will-be-defined = [//] @a-mixed-list; # [//] will return the first defined value

my @a = Any,Any,1,1;
my @b = 2,Any,Any,2;
my @may-contain-any = @a >>//<< @b; # contains: [2, Any, 1, 1]

sub f1(){Failure.new('WELP!')};
sub f2(){ $_ ~~ Failure }; # orelse will kindly set the topic for us
my $s = (f1() orelse f2()); # Please note the parentheses, which are needed because orelse is
                            # much looser then infix:<=> .
dd $s; # this be Bool::False
```


Finally, another major group of undefined values represents failures.  Raku is designed with the notion that throwing exceptions is bad for parallel processing, so exceptions are thrown lazily; that is, they tend to be returned in-band instead as data, and are only thrown for real if not properly handled as exception data.  (In which case, the exception is required to report the original difficulty accurately.)  In order not to disrupt control flow and the synchronicity of event ordering, such failures are returned in-band as a normal values.  And in order not to subvert the type system when it comes to return types, failure may be mixed into any reference type which produces an undefined, typed value which is also indicates the nature of the failure.



Native storage types work under different rules, since most native types cannot represent failure, or even undefinedness.  Any attempt to assign a value to a storage location that cannot represent a failure will cause an exception to be thrown.
