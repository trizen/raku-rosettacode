[1]: https://rosettacode.org/wiki/Call_a_function

# [Call a function][1]





### Theory



Fundamentally, nearly everything you do in Raku is a function call if you look hard enough.
At the lowest level, a function call merely requires a reference to any
kind of invokable object, and a call to its `postcircumfix:<( )>` method.
However, there are various forms of sugar and indirection that you
can use to express these function calls differently.  In particular,
operators are all just sugar for function calls.



Calling a function that requires no arguments:

```perl
foo               # as list operator
foo()             # as function
foo.()            # as function, explicit postfix form
$ref()            # as object invocation
$ref.()           # as object invocation, explicit postfix
&foo()            # as object invocation
&foo.()           # as object invocation, explicit postfix
::($name)()       # as symbolic ref
```


Calling a function with exactly one argument:

```perl
foo 1             # as list operator
foo(1)            # as named function
foo.(1)           # as named function, explicit postfix
$ref(1)           # as object invocation (must be hard ref)   
$ref.(1)          # as object invocation, explicit postfix
1.$foo            # as pseudo-method meaning $foo(1) (hard ref only)
1.$foo()          # as pseudo-method meaning $foo(1) (hard ref only)
1.&foo            # as pseudo-method meaning &foo(1) (is hard foo)
1.&foo()          # as pseudo-method meaning &foo(1) (is hard foo)
1.foo             # as method via dispatcher
1.foo()           # as method via dispatcher
1."$name"()       # as method via dispatcher, symbolic
+1                # as operator to prefix:<+> function
```


Method calls are included here because they do eventually dispatch to a true
function via a dispatcher.  However, the dispatcher in question is not going
to dispatch to the same set of functions that a function call of that name
would invoke.  That's why there's a dispatcher, after all.  Methods are declared
with a different keyword, `method`, in Raku, but all that does is
install the actual function into a metaclass.  Once it's there, it's merely
a function that expects its first argument to be the invocant object.  Hence we
feel justified in including method call syntax as a form of indirect function call.



Operators like `+` also go through a dispatcher, but in this case it is
multiply dispatched to all lexically scoped candidates for the function.  Hence
the candidate list is bound early, and the function itself can be bound early
if the type is known.  Raku maintains a clear distinction between early-bound
linguistic constructs that force Perlish semantics, and late-bound OO dispatch
that puts the objects and/or classes in charge of semantics.  (In any case, `&foo`,
though being a hard ref to the function named "foo", may actually be a ref to
a dispatcher to a list of candidates that, when called, makes all the candidates behave as a single unit.)



Calling a function with exactly two arguments:

```perl
foo 1,2           # as list operator
foo(1,2)          # as named function
foo.(1,2)         # as named function, explicit postfix
$ref(1,2)         # as object invocation (must be hard ref)
$ref.(1,2)        # as object invocation, explicit postfix
1.$foo: 2         # as pseudo-method meaning $foo(1,2) (hard ref only)
1.$foo(2)         # as pseudo-method meaning $foo(1,2) (hard ref only)
1.&foo: 2         # as pseudo-method meaning &foo(1,2) (is hard foo)
1.&foo(2)         # as pseudo-method meaning &foo(1,2) (is hard foo)
1.foo: 2          # as method via dispatcher
1.foo(2)          # as method via dispatcher
1."$name"(2)      # as method via dispatcher, symbolic
1 + 2             # as operator to infix:<+> function
```


Optional arguments don't look any different from normal arguments.
The optionality is all on the binding end.



Calling a function with a variable number of arguments (varargs):

```perl
foo @args         # as list operator
foo(@args)        # as named function
foo.(@args)       # as named function, explicit postfix
$ref(@args)       # as object invocation (must be hard ref)
$ref.(@args)      # as object invocation, explicit postfix
1.$foo: @args     # as pseudo-method meaning $foo(1,@args) (hard ref)
1.$foo(@args)     # as pseudo-method meaning $foo(1,@args) (hard ref)
1.&foo: @args     # as pseudo-method meaning &foo(1,@args)
1.&foo(@args)     # as pseudo-method meaning &foo(1,@args)
1.foo: @args      # as method via dispatcher
1.foo(@args)      # as method via dispatcher
1."$name"(@args)  # as method via dispatcher, symbolic
@args X @blargs   # as list infix operator to infix:<X>
```


Note: whether a function may actually be called with a variable number of arguments depends entirely
on whether a signature accepts a list at that position in the argument list, but
describing that is not the purpose of this task.  Suffice to say that we assume here that the
foo function is declared with a signature of the form (\*@params).  The calls above might be interpreted as having a single array argument if the signature indicates a normal parameter instead of a variadic one.  What you cannot do in Raku (unlike Perl 5) is pass an array as several fixed arguments.  By default it must either represent a single argument, or be part of a variadic list.  You can force the extra level of argument list interpolation using a prefix `|` however:

```perl
my @args = 1,2,3;
foo(|@args);  # equivalent to foo(1,2,3)
```


Calling a function with named arguments:

```perl
foo :a, :b(4), :!c, d => "stuff"
foo(:a, :b(4), :!c, d => "stuff")
```


...and so on.  Operators may also be called with named arguments, but only 
colon adverbials are allowed:

```perl
1 + 1 :a :b(4) :!c :d("stuff")   # calls infix:<+>(1,1,:a, :b(4), :!c, d => "stuff")
```


Using a function in statement context:

```perl
foo(); bar(); baz();    # evaluate for side effects
```


Using a function in first class context within an expression:

```perl
1 / find-a-func(1,2,3)(4,5,6) ** 2;
```


Obtaining the return value of a function:

```perl
my $result = somefunc(1,2,3) + 2;
```


There is no difference between calling builtins and user-defined functions and operators (or
even control stuctures).  This was a major design goal of Raku, and apart from a very few
low-level primitives, all of Raku can be written in Raku.



There is no difference between calling subroutines and functions in Raku, other than that
calling a function in void context that has no side effects is likely to get you a "Useless use of..." warning.
And, of course, the fact that pure functions can participate in more optimizations such as constant folding.



By default, arguments are passed readonly, which allows the implementation to decide whether pass-by-reference or pass-by-value is more efficient on a case-by-case basis.  Explicit lvalue, reference, or copy semantics may be requested on a parameter-by-parameter basis, and the entire argument list may be processed raw if that level of control is needed.



### Practice



Demonstrating each of the above-mentioned function calls with actual running code, along with the various extra definitions required to make them work (in certain cases).  Arguments are checked, and function name / run-sequence number are displayed upon success.

```perl
{
state $n;

multi f ()          {                                    print ' f' ~ ++$n }
multi f ($a)        { die if 1  != $a;                   print ' f' ~ ++$n }
multi f ($a,$b)     { die if 3  != $a+$b;                print ' f' ~ ++$n }
multi f (@a)        { die if @a != [2,3,4];              print ' f' ~ ++$n }
multi f ($a,$b,$c)  { die if 2  != $a || 4 != $c;        print ' f' ~ ++$n }
sub   g ($a,*@b)    { die if @b != [2,3,4] || 1 != $a;   print ' g' ~ ++$n }

my \i = ->          {                                    print ' i' ~ ++$n }
my \l = -> $a       { die if 1 != $a;                    print ' l' ~ ++$n }
my \m = -> $a,$b    { die if 1 != $a || 2 != $b;         print ' m' ~ ++$n }
my \n = -> @a       { die if @a != [2,3,4];              print ' n' ~ ++$n }

Int.^add_method( 'j', method ()
                    { die if 1 != self;                  print ' j' ~ ++$n } );
Int.^add_method( 'k', method ($a)
                    { die if 1 != self || 2 != $a;       print ' k' ~ ++$n } );
Int.^add_method( 'h', method (@a)
                    { die if @a != [2,3,4] || 1 != self; print ' h' ~ ++$n } );

my $ref   =  &f;  # soft ref
my $f    :=  &f;  # hard ref
my $g    :=  &g;  # hard ref
my $f-sym = '&f'; # symbolic ref
my $g-sym = '&g'; # symbolic ref
my $j-sym =  'j'; # symbolic ref
my $k-sym =  'k'; # symbolic ref
my $h-sym =  'h'; # symbolic ref

# Calling a function with no arguments:

f;            #  1  as list operator
f();          #  2  as function
i.();         #  3  as function, explicit postfix form  # defined via pointy-block
$ref();       #  4  as object invocation
$ref.();      #  5  as object invocation, explicit postfix
&f();         #  6  as object invocation
&f.();        #  7  as object invocation, explicit postfix
::($f-sym)(); #  8  as symbolic ref

# Calling a function with exactly one argument:

f 1;          #   9  as list operator
f(1);         #  10  as named function
l.(1);        #  11  as named function, explicit postfix  # defined via pointy-block
$f(1);        #  12  as object invocation (must be hard ref)
$ref.(1);     #  13  as object invocation, explicit postfix
1.$f;         #  14  as pseudo-method meaning $f(1) (hard ref only)
1.$f();       #  15  as pseudo-method meaning $f(1) (hard ref only)
1.&f;         #  16  as pseudo-method meaning &f(1) (is hard f)
1.&f();       #  17  as pseudo-method meaning &f(1) (is hard f)
1.j;          #  18  as method via dispatcher             # requires custom method, via 'Int.^add_method'
1.j();        #  19  as method via dispatcher
1."$j-sym"(); #  20  as method via dispatcher, symbolic

# Calling a function with exactly two arguments:

f 1,2;         #  21  as list operator
f(1,2);        #  22  as named function
m.(1,2);       #  23  as named function, explicit postfix  # defined via pointy-block
$ref(1,2);     #  24  as object invocation (must be hard ref)
$ref.(1,2);    #  25  as object invocation, explicit postfix
1.$f: 2;       #  26  as pseudo-method meaning $f(1,2) (hard ref only)
1.$f(2);       #  27  as pseudo-method meaning $f(1,2) (hard ref only)
1.&f: 2;       #  28  as pseudo-method meaning &f(1,2) (is hard f)
1.&f(2);       #  29  as pseudo-method meaning &f(1,2) (is hard f)
1.k: 2;        #  30  as method via dispatcher             # requires custom method, via 'Int.^add_method'
1.k(2);        #  31  as method via dispatcher
1."$k-sym"(2); #  32  as method via dispatcher, symbolic

# Calling a function with a variable number of arguments (varargs):

my @args = 2,3,4;

f @args;           #  33  as list operator
f(@args);          #  34  as named function
n.(@args);         #  35  as named function, explicit postfix   # defined via pointy-block
$ref(@args);       #  36  as object invocation (must be hard ref)
$ref.(@args);      #  37  as object invocation, explicit postfix
1.$g: @args;       #  38  as pseudo-method meaning $f(1,@args) (hard ref)
1.$g(@args);       #  39  as pseudo-method meaning $f(1,@args) (hard ref)
1.&g: @args;       #  40  as pseudo-method meaning &f(1,@args)
1.&g(@args);       #  41  as pseudo-method meaning &f(1,@args)
1.h: @args;        #  42  as method via dispatcher              # requires custom method, via 'Int.^add_method'
1.h(@args);        #  43  as method via dispatcher
1."$h-sym"(@args); #  44  as method via dispatcher, symbolic
f(|@args);         #  45  equivalent to f(1,2,3)

}
```

#### Output:
```
f1 f2 i3 f4 f5 f6 f7 f8 f9 f10 l11 f12 f13 f14 f15 f16 f17 j18 j19 j20 f21 f22 m23 f24 f25 f26 f27 f28 f29 k30 k31 k32 f33 f34 n35 f36 f37 g38 g39 g40 g41 h42 h43 h44 f45
```
