[1]: http://rosettacode.org/wiki/Non-continuous_subsequences

# [Non-continuous subsequences][1]

```perl6
sub non_continuous_subsequences ( *@list ) {
    @list.combinations.grep: { 1 != all( .[ 0 ^.. .end] Z- .[0 ..^ .end] ) }
}
 
say non_continuous_subsequences( 1..3 )».gist;
say non_continuous_subsequences( 1..4 )».gist;
say non_continuous_subsequences(   ^4 ).map: {[<a b c d>[.list]].gist};
```

#### Output:
```
((1 3))
((1 3) (1 4) (2 4) (1 2 4) (1 3 4))
([a c] [a d] [b d] [a b d] [a c d])
```