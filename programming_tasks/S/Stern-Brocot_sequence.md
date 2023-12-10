[1]: https://rosettacode.org/wiki/Stern-Brocot_sequence

# [Stern-Brocot sequence][1]



```perl
constant @Stern-Brocot = 1, 1, { |(@_[$_ - 1] + @_[$_], @_[$_]) given ++$ } ... *;
 
put @Stern-Brocot[^15];
 
for flat 1..10, 100 -> $ix {
    say "First occurrence of {$ix.fmt('%3d')} is at index: {(1+@Stern-Brocot.first($ix, :k)).fmt('%4d')}";
}
 
say so 1 == all map ^1000: { [gcd] @Stern-Brocot[$_, $_ + 1] }
```

#### Output:
```
1 1 2 1 3 2 3 1 4 3 5 2 5 3 4
First occurrence of   1 is at index:    1
First occurrence of   2 is at index:    3
First occurrence of   3 is at index:    5
First occurrence of   4 is at index:    9
First occurrence of   5 is at index:   11
First occurrence of   6 is at index:   33
First occurrence of   7 is at index:   19
First occurrence of   8 is at index:   21
First occurrence of   9 is at index:   35
First occurrence of  10 is at index:   39
First occurrence of 100 is at index: 1179
True
```
