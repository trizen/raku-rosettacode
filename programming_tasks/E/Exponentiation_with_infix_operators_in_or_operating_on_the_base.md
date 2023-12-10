[1]: https://rosettacode.org/wiki/Exponentiation_with_infix_operators_in_(or_operating_on)_the_base

# [Exponentiation with infix operators in (or operating on) the base][1]

In Raku by default, infix exponentiation binds tighter than unary negation. It is trivial however to define your own infix operators with whatever precedence level meets the needs of your program.



A slight departure from the task specs. Use `1 + {expression}` rather than just `{expression}` to better demonstrate the relative precedence levels. Where `{expression}` is one of:



Also add a different grouping: `(1 + -x){exponential operator}p`

```perl
sub infix:<↑> is looser(&prefix:<->) { $^a ** $^b }
sub infix:<∧> is looser(&infix:<->)  { $^a ** $^b }

for
   ('Default precedence: infix exponentiation is tighter (higher) precedence than unary negation.',
     '1 + -$x**$p',   {1 + -$^a**$^b},   '1 + (-$x)**$p', {1 + (-$^a)**$^b},  '1 + (-($x)**$p)', {1 + (-($^a)**$^b)},
     '(1 + -$x)**$p', {(1 + -$^a)**$^b}, '1 + -($x**$p)', {1 + -($^a**$^b)}),

   ("\nEasily modified: custom loose infix exponentiation is looser (lower) precedence than unary negation.",
     '1 + -$x↑$p ',   {1 + -$^a↑$^b},    '1 + (-$x)↑$p ', {1 + (-$^a)↑$^b},   '1 + (-($x)↑$p) ', {1 + (-($^a)↑$^b)},
     '(1 + -$x)↑$p ', {(1 + -$^a)↑$^b},  '1 + -($x↑$p) ', {1 + -($^a↑$^b)}),

   ("\nEven more so: custom looser infix exponentiation is looser (lower) precedence than infix subtraction.",
     '1 + -$x∧$p ',   {1 + -$^a∧$^b},    '1 + (-$x)∧$p ', {1 + (-$^a)∧$^b},   '1 + (-($x)∧$p) ', {1 + (-($^a)∧$^b)},
     '(1 + -$x)∧$p ', {(1 + -$^a)∧$^b},  '1 + -($x∧$p) ', {1 + -($^a∧$^b)})
-> $case {
    my ($title, @operations) = $case<>;
    say $title;
    for -5, 5 X 2, 3 -> ($x, $p) {
        printf "x = %2d  p = %d", $x, $p;
        for @operations -> $label, &code { print " │ $label = " ~ $x.&code($p).fmt('%4d') }
        say ''
    }
}
```

#### Output:
```
Default precedence: infix exponentiation is tighter (higher) precedence than unary negation.
x = -5  p = 2 │ 1 + -$x**$p =  -24 │ 1 + (-$x)**$p =   26 │ 1 + (-($x)**$p) =  -24 │ (1 + -$x)**$p =   36 │ 1 + -($x**$p) =  -24
x = -5  p = 3 │ 1 + -$x**$p =  126 │ 1 + (-$x)**$p =  126 │ 1 + (-($x)**$p) =  126 │ (1 + -$x)**$p =  216 │ 1 + -($x**$p) =  126
x =  5  p = 2 │ 1 + -$x**$p =  -24 │ 1 + (-$x)**$p =   26 │ 1 + (-($x)**$p) =  -24 │ (1 + -$x)**$p =   16 │ 1 + -($x**$p) =  -24
x =  5  p = 3 │ 1 + -$x**$p = -124 │ 1 + (-$x)**$p = -124 │ 1 + (-($x)**$p) = -124 │ (1 + -$x)**$p =  -64 │ 1 + -($x**$p) = -124

Easily modified: custom loose infix exponentiation is looser (lower) precedence than unary negation.
x = -5  p = 2 │ 1 + -$x↑$p  =   26 │ 1 + (-$x)↑$p  =   26 │ 1 + (-($x)↑$p)  =   26 │ (1 + -$x)↑$p  =   36 │ 1 + -($x↑$p)  =  -24
x = -5  p = 3 │ 1 + -$x↑$p  =  126 │ 1 + (-$x)↑$p  =  126 │ 1 + (-($x)↑$p)  =  126 │ (1 + -$x)↑$p  =  216 │ 1 + -($x↑$p)  =  126
x =  5  p = 2 │ 1 + -$x↑$p  =   26 │ 1 + (-$x)↑$p  =   26 │ 1 + (-($x)↑$p)  =   26 │ (1 + -$x)↑$p  =   16 │ 1 + -($x↑$p)  =  -24
x =  5  p = 3 │ 1 + -$x↑$p  = -124 │ 1 + (-$x)↑$p  = -124 │ 1 + (-($x)↑$p)  = -124 │ (1 + -$x)↑$p  =  -64 │ 1 + -($x↑$p)  = -124

Even more so: custom looser infix exponentiation is looser (lower) precedence than infix subtraction.
x = -5  p = 2 │ 1 + -$x∧$p  =   36 │ 1 + (-$x)∧$p  =   36 │ 1 + (-($x)∧$p)  =   26 │ (1 + -$x)∧$p  =   36 │ 1 + -($x∧$p)  =  -24
x = -5  p = 3 │ 1 + -$x∧$p  =  216 │ 1 + (-$x)∧$p  =  216 │ 1 + (-($x)∧$p)  =  126 │ (1 + -$x)∧$p  =  216 │ 1 + -($x∧$p)  =  126
x =  5  p = 2 │ 1 + -$x∧$p  =   16 │ 1 + (-$x)∧$p  =   16 │ 1 + (-($x)∧$p)  =   26 │ (1 + -$x)∧$p  =   16 │ 1 + -($x∧$p)  =  -24
x =  5  p = 3 │ 1 + -$x∧$p  =  -64 │ 1 + (-$x)∧$p  =  -64 │ 1 + (-($x)∧$p)  = -124 │ (1 + -$x)∧$p  =  -64 │ 1 + -($x∧$p)  = -124
```
