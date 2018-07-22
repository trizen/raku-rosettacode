[1]: https://rosettacode.org/wiki/Count_occurrences_of_a_substring

# [Count occurrences of a substring][1]

```perl
sub count-substring($big,$little) { +$big.comb: ~$little }
 
say count-substring("the three truths","th"); # 3
say count-substring("ababababab","abab");     # 4
 
say count-substring(123123123,12);            # 3
```


The `~` prefix operator converts `$little` to a `Str` if it isn't already, and `.comb` when given a `Str` as an argument returns instances of that substring. You can think of it as if the argument was a regex that matched the string literally `/$little/`. Also, prefix `+` forces numeric context in Perl&#160;6 (it's a no-op in Perl&#160;5). For the built in listy types that is the same as calling `.elems` method. One other style point: we now tend to prefer hyphenated names over camelCase.