[1]: https://rosettacode.org/wiki/Function_composition

# [Function composition][1]

The function composition operator is `∘`, U+2218 RING OPERATOR (with a "Texas" version `o` for the Unicode challenged). Here we compose a routine, an operator, and a lambda:

```raku
sub triple($n) { 3 * $n }
my &f = &triple ∘ &prefix:<-> ∘ { $^n + 2 };
say &f(5); # prints "-21".
```