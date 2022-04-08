[1]: https://rosettacode.org/wiki/Overloaded_operators

# [Overloaded operators][1]

While it is very easy to overload operators in Raku, it isn't really common...
at least, not in the traditional sense. Or it's extremely common... It depends on how you view it.



One of the founding principles of Raku is that: "Different things should look different". It follows that "Similar things should look similar".



To pick out one tiny example: Adding numbery things together shouldn't be easily confusable with concatenating
strings. Instead, Raku has the "concatenation" operator: ~ for joining stringy
things together.



Using a numeric-ish operator implies that you want a numeric-ish answer... so Raku
will try very hard to give you what you ask for, no matter what operands you
pass it.



Raku operators have multiple candidates to try to fulfil your request and will
try to coerce the operands to a sensible value.



Addition:

```perl
say  3    +  5;      # Int plus Int
say  3.0  +  0.5e1;  # Rat plus Num
say '3'   +  5;      # Str plus Int
say  3    + '5';     # Int plus Str
say '3'   + '5';     # Str plus Str
say '3.0' + '0.5e1'; # Str plus Str
say (2, 3, 4) + [5, 6]; # List plus Array
```


+ is a numeric operator so every thing is evaluated numerically if possible


#### Output:
```
8
8
8
8
8
8
5 # a list or array evaluated numerically returns the number of elements
```




Concatenation:

```perl
say  3    ~  5;      # Int concatenate Int
say  3.0  ~  0.5e1;  # Rat concatenate Num
say '3'   ~  5;      # Str concatenate Int
say  3    ~ '5';     # Int concatenate Str
say '3'   ~ '5';     # Str concatenate Str
say '3.0' ~ '0.5e1'; # Str concatenate Str
say (2, 3, 4) ~ [5, 6]; # List concatenate Array
```


~ is a Stringy operator so everything is evaluated as a string (numerics are evaluated numerically then coerced to a string).


#### Output:
```
35
35
35
35
35
3.00.5e1
2 3 45 6 # default stringification, then concatenate
```


There is nothing preventing you from overloading or overriding existing
operators. Raku firmly believes in not putting pointless restrictions on
what you can and can not do. Why make it hard to do the "wrong" thing when
we make it so easy to do it right?



There is no real impetus to "overload" existing operators to do *different*
things, it is **very** easy to add new operators in Raku, and nearly **any**
Unicode character or combination may used to define it. They may be infix,
prefix, postfix, (or post-circumfix!) The precedence, associativity and arity
are all easily defined. An operator at heart is just a subroutine with funny calling conventions.



Borrowed from the [Nimber arithmetic](https://rosettacode.org/wiki/Nimber_arithmetic#Raku) task:



New operators, defined in place. Arity is two (almost all infix operators
take two arguments), precedence is set equivalent to similar existing operators,
default (right) associativity. The second, ⊗, actually uses itself to define
itself.

```perl
sub infix:<⊕> (Int $x, Int $y) is equiv(&infix:<+>) { $x +^ $y }
 
sub infix:<⊗> (Int $x, Int $y) is equiv(&infix:<×>) {
    return $x × $y if so $x|$y < 2;
    my $h = exp $x.lsb, 2;
    return $h ⊗ $y ⊕ (($x ⊕ $h) ⊗ $y) if $x > $h;
    return $y ⊗ $x if $y.lsb < $y.msb;
    return $x × $y unless my $comp = $x.lsb +& $y.lsb;
    $h = exp $comp.lsb, 2;
    (($x +> $h) ⊗ ($y +> $h)) ⊗ (3 +< ($h - 1))
}
 
say 123 ⊗ 456;
```

#### Output:
```
31562
```




That's all well and good, but suppose you have a new class, say, a Line class, and
you want to be able to do arithmetic on Lines. No need to override the built
in arithmetic operators, just add a multi candidate to do the right thing. A multi
allows *adding* a new definition of the operator without disturbing the existing ones.



Very, **very** basic Line class:

```perl
class Line {
    has @.start;
    has @.end;
}
 
# New infix + multi to add two Lines together, for some bogus definition of add
multi infix:<+> (Line $x, Line $y) {
    Line.new(
       :start(
          sqrt($x.start[0]² + $y.start[0]²),
          sqrt($x.start[1]² + $y.start[1]²)
       ),
       :end(
          sqrt($x.end[0]² + $y.end[0]²),
          sqrt($x.end[1]² + $y.end[1]²)
       )
    )
}
 
# In operation:
say Line.new(:start(-4,7), :end(5,0)) + Line.new(:start(1,1), :end(2,3));
```

#### Output:
```
Line.new(start => [4.123105625617661e0, 7.0710678118654755e0], end => [5.385164807134504e0, 3e0])
```


To be fair, all of this easy power in a bad programmers hands can lead to incomprehensible code... but bad programmers can be bad in any language.
