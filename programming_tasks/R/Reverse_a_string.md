[1]: http://rosettacode.org/wiki/Reverse_a_string

# [Reverse a string][1]

Perl 6 handles graphemes correctly by default.

```perl6
say "hello world".flip;
say "as⃝df̅".flip
```

#### Output:
```
dlrow olleh
f̅ds⃝a
```