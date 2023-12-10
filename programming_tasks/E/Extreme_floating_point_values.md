[1]: https://rosettacode.org/wiki/Extreme_floating_point_values

# [Extreme floating point values][1]





Floating point limits are to a large extent implementation dependent, but currently both Raku backends (MoarVM, JVM) running on a 64 bit OS have an infinity threshold of just under 1.8e308.

```perl
print qq:to 'END'
positive infinity: {1.8e308}
negative infinity: {-1.8e308}
negative zero: {0e0 * -1}
not a number: {0 * 1e309}
+Inf + 2.0 = {Inf + 2}
+Inf - 10.1 = {Inf - 10.1}
0 * +Inf = {0 * Inf}
+Inf + -Inf = {Inf + -Inf}
+Inf == -Inf = {+Inf == -Inf}
(-Inf+0i)**.5 = {(-Inf+0i)**.5}
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


```
positive infinity: Inf
negative infinity: -Inf
negative zero: -0
not a number: NaN
+Inf + 2.0 = Inf
+Inf - 10.1 = Inf
0 * +Inf = NaN
+Inf + -Inf = NaN
+Inf == -Inf = False
(-Inf+0i)**.5 = Inf+Inf\i
NaN + 1.0 = NaN
NaN + NaN = NaN
NaN == NaN = False
0.0 == -0.0 = True
```
