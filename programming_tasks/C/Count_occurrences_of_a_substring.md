[1]: http://rosettacode.org/wiki/Count_occurrences_of_a_substring

# [Count occurrences of a substring][1]

```perl
sub count-substring($big,$little) { +$big.comb: /$little/ }
 
say count-substring("the three truths","th");
say count-substring("ababababab","abab");
```


Note that in Perl 6 the `/$little/` matches the variable literally, so there's no need to quote regex metacharacters. Also, prefix `+` forces numeric context in Perl&#160;6 (it's a no-op in Perl&#160;5). One other style point: we now tend to prefer hyphenated names over camelCase.