[1]: http://rosettacode.org/wiki/Strip_comments_from_a_string

# [Strip comments from a string][1]

```perl6
$*IN.slurp.subst(/ \h* <[ # ; ]> \N* /, '', :g).print
```