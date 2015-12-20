[1]: http://rosettacode.org/wiki/Loops/Nested

# [Loops/Nested][1]

```perl
my @a = [ (1..20).roll(10) ] xx *;
 
LINE: for @a -> @line {
    for @line -> $elem {
        print " $elem";
        last LINE if $elem == 20;
    }
    print "\n";
}
print "\n";
```

#### Output:
```
 15 6 14 13 14 7 9 16 8 18
 7 6 18 11 19 13 12 5 18 8
 17 17 9 5 4 8 17 8 3 11
 9 20
```