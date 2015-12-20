[1]: http://rosettacode.org/wiki/Look-and-say_sequence

# [Look-and-say sequence][1]

In Perl 6 it is natural to avoid explicit loops; rather we use the sequence operator to define a lazy sequence. We print all values until we interrupt with Ctrl-C.

```perl
.say for '1', *.subst(/(.)$0*/, { .chars ~ .[0] }, :g) ... *;
```

#### Output:
```
1
11
21
1211
111221
312211
13112221
1113213211
31131211131221
13211311123113112211
^C
```