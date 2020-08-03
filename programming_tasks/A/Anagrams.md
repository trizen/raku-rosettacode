[1]: https://rosettacode.org/wiki/Anagrams

# [Anagrams][1]

```raku
my @anagrams = 'unixdict.txt'.IO.words.classify(*.comb.sort.join).values;
 
my $max = @anagrams».elems.max;
 
.put for @anagrams.grep(*.elems == $max);
```

#### Output:
```
caret carte cater crate trace
angel angle galen glean lange
alger glare lager large regal
elan lane lean lena neal
evil levi live veil vile
abel able bale bela elba
```


Just for the fun of it, here's a one-liner that uses no temporaries. Since it would be rather long, we've oriented it vertically:

```raku
.put for                         # print each element of the array made this way:
    'unixdict.txt'.IO.words      # load words from file
    .classify(*.comb.sort.join)  # group by common anagram
    .classify(*.value.elems)     # group by number of anagrams in a group
    .max(*.key).value            # get the group with highest number of anagrams
    .map(*.value)                # get all groups of anagrams in the group just selected
```