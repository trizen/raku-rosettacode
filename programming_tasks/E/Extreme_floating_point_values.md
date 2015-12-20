[1]: http://rosettacode.org/wiki/Extreme_floating_point_values

# [Extreme floating point values][1]

```perl
print qq:to 'END'
positive infinity: {Inf}
negative infinity: {-Inf}
negative zero: {-0e0}
not a number: {NaN}
+Inf + 2.0 = {Inf + 2}
+Inf - 10.1 = {Inf - 10.1}
+Inf + -Inf = {Inf + -Inf}
0 * +Inf = {0 * Inf}
NaN + 1.0 = {NaN + 1.0}
NaN + NaN = {NaN + NaN}
NaN == NaN = {NaN == NaN}
0.0 == -0.0 = {0e0 == -0e0}
END
```


`0e0` is used to have floating point number.
Simply using `0.0` makes rational number that doesn't recognize `-0`.
`qq:to` is heredoc syntax, where `qq` means
that variables and closures (between braces) are interpolated.


#### Output:
```
positive infinity: Inf
negative infinity: -Inf
negative zero: -0
not a number: NaN
+Inf + 2.0 = Inf
+Inf - 10.1 = Inf
+Inf + -Inf = NaN
0 * +Inf = NaN
NaN + 1.0 = NaN
NaN + NaN = NaN
NaN == NaN = False
0.0 == -0.0 = True
```