[1]: https://rosettacode.org/wiki/Loops/Infinite

# [Loops/Infinite][1]



```perl
loop {
    say 'SPAM';
}
```


In addition, there are various ways of writing lazy, infinite lists in Raku:

```perl
print "SPAM\n" xx *;      # repetition operator
print "SPAM\n", ~* ... *; # sequence operator
map {say "SPAM"}, ^Inf;   # upto operator
```
