[1]: https://rosettacode.org/wiki/Monads/Maybe_monad

# [Monads/Maybe monad][1]

It is exceptionally difficult to come up with a compelling valuable use for
a Maybe monad in Perl 6. Monads are most useful in languages that don't have
exceptions and/or only allow a single point of entry/exit from a subroutine.



There are very good reasons to have those restrictions. It makes it much less
complex to reason about a programs correctness and actually *prove* that a
program is correct, but those restrictions also lead to oddities like Monads,
just as a way to work around those restrictions.



The task description asks for two functions that take an Int and return a Maybe
Int or Maybe Str, but those distinctions are nearly meaningless in Perl 6. See
below.

```perl
my $monad = <42>;
say 'Is $monad an Int?: ', $monad ~~ Int;
say 'Is $monad a  Str?: ', $monad ~~ Str;
say 'Wait, what? What exactly is $monad?: ', $monad.^name;
```

#### Output:
```
Is $monad an Int?: True
Is $monad a  Str?: True
Wait, what? What exactly is $monad?: IntStr
```


$monad is both an Int **and** a Str. It exists in a sort-of quantum state
where what-it-is depends on how-it-is-used. Bolting on some 'Monad' type to do
this will only *remove* functionality that Perl 6 already has.



Ok, say you have a situation where you absolutely do not want an incalculable
value to halt processing. (In a web process perhaps.) In that case, it *might*
be useful to subvert the normal exception handler and just return a "Nothing"
type. Any routine that needs to be able to handle a Nothing type will need to be
informed of it, but for the most part, that just means adding a multi-dispatch
candidate.

```perl
# Build a Nothing type. When treated as a string it returns the string 'Nothing'.
# When treated as a Numeric, returns the value 'Nil'.
class NOTHING {
    method Str { 'Nothing' }
    method Numeric { Nil }
}
 
# A generic instance of a Nothing type.
my \Nothing = NOTHING.new;
 
# A reimplementation of the square-root function. Could just use the CORE one
# but this more fully shows how multi-dispatch candidates are added.
 
# Handle positive numbers & 0
multi root (Numeric $n where * >= 0) { $n.sqrt }
# Handle Negative numbers (Complex number handling is built in.)
multi root (Numeric $n where * <  0) { $n.Complex.sqrt }
# Else return Nothing
multi root ($n) { Nothing }
 
# Handle numbers > 0
multi ln (Real $n where * > 0) { log $n, e }
# Else return Nothing
multi ln ($n) { Nothing }
 
# Handle fraction where the denominator != 0
multi recip (Numeric $n where * != 0) { 1/$n }
# Else return Nothing
multi recip ($n) { Nothing }
 
# Helper formatting routine
sub center ($s) {
    my $pad = 21 - $s.Str.chars;
    ' ' x ($pad / 2).floor ~ $s ~ ' ' x ($pad / 2).ceiling;
}
 
# Display the "number" the reciprocal, the root, natural log and the 3 functions
# composed together.
put ('"Number"', 'Reciprocal', 'Square root', 'Natural log', 'Composed')».&center;
 
# Note how it handles the last two "values". The string 'WAT' is not numeric at
# all; but Ethiopic number 30, times vulgar fraction 1/7, is.
put ($_, .&recip, .&root, .&ln, .&(&ln o &root o &recip) )».&center
  for -2, -1, -0.5, 0, exp(-1), 1, 2, exp(1), 3, 4, 5, 'WAT', ፴ × ⅐;
```

#### Output:
```
      "Number"             Reciprocal            Square root           Natural log            Composed       
         -2                   -0.5          0+1.4142135623730951i        Nothing               Nothing       
         -1                    -1                   0+1i                 Nothing               Nothing       
        -0.5                   -2           0+0.7071067811865476i        Nothing               Nothing       
          0                  Nothing                  0                  Nothing               Nothing       
 0.36787944117144233    2.718281828459045    0.6065306597126334            -1                    0.5         
          1                     1                     1                     0                     0          
          2                    0.5           1.4142135623730951    0.6931471805599453    -0.3465735902799726 
  2.718281828459045    0.36787944117144233   1.6487212707001282             1                   -0.5         
          3                 0.333333         1.7320508075688772    1.0986122886681098    -0.5493061443340549 
          4                   0.25                    2            1.3862943611198906    -0.6931471805599453 
          5                    0.2            2.23606797749979     1.6094379124341003    -0.8047189562170503 
         WAT                 Nothing               Nothing               Nothing               Nothing       
      4.285714              0.233333         2.0701966780270626     1.455287232606842    -0.727643616303421
```
