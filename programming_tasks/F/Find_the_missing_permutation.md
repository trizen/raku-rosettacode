[1]: https://rosettacode.org/wiki/Find_the_missing_permutation

# [Find the missing permutation][1]

```raku
my @givens = <ABCD CABD ACDB DACB BCDA ACBD ADCB CDAB DABC BCAD CADB CDBA
                CBAD ABDC ADBC BDCA DCBA BACD BADC BDAC CBDA DBCA DCAB>;
 
my @perms = <A B C D>.permutations.map: *.join;
 
.say when none(@givens) for @perms;
```

#### Output:
```
DBAC
```


Of course, all of these solutions are working way too hard,
when you can just xor all the bits,
and the missing one will just pop right out:

```raku
say [~^] <ABCD CABD ACDB DACB BCDA ACBD ADCB CDAB DABC BCAD CADB CDBA
          CBAD ABDC ADBC BDCA DCBA BACD BADC BDAC CBDA DBCA DCAB>;
```

#### Output:
```
DBAC
```