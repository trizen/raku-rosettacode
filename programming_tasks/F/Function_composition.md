[1]: http://rosettacode.org/wiki/Function_composition

# [Function composition][1]

The function composition operator is <tt>∘</tt>, U+2218 RING OPERATOR (with a "Texas" version <tt>o</tt> for the Unicode challenged). Here we compose a routine, an operator, and a lambda:

```perl6
sub triple($n) { 3 * $n }
my &f = &triple ∘ &prefix:<-> ∘ { $^n + 2 };
say &f(5); # Prints "-21".
```