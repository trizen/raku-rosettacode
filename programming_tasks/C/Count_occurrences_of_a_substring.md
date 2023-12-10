[1]: https://rosettacode.org/wiki/Count_occurrences_of_a_substring

# [Count occurrences of a substring][1]



```perl
sub count-substring($big, $little) { +$big.comb: / :r $little / }
 
say count-substring("the three truths", "th"); # 3
say count-substring("ababababab", "abab");     # 2
 
say count-substring(123123123, 12);            # 3
```


The `:r` adverb makes the regex "ratchet forward" and skip any overlapping matches. `.comb` - when given a `Regex` as an argument - returns instances of that substring. Also, prefix `+` forces numeric context in Raku (it's a no-op in Perl&#160;5).  For the built in listy types that is the same as calling `.elems` method.  One other style point: we now tend to prefer hyphenated names over camelCase.
