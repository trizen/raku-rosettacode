[1]: http://rosettacode.org/wiki/Anagrams/Deranged_anagrams

# [Anagrams/Deranged anagrams][1]

Note that, to make runtime manageable, we have created a subset file:

```bash
grep '^[ie]' unixdict.txt > dict.ie
```
```perl6
my %anagram = slurp('dict.ie').words.map({[.comb]}).classify({ .sort.join });
 
for %anagram.values.sort({ -@($_[0]) }) -> @aset {
    for     0   ..^ @aset.end -> $i {
        for $i ^..  @aset.end -> $j {
            if none(  @aset[$i].list Zeq @aset[$j].list ) {
                say "{@aset[$i].join}   {@aset[$j].join}";
                exit;
            }
        }
    }
}
```

#### Output:
```
excitation   intoxicate
```