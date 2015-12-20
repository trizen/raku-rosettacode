[1]: http://rosettacode.org/wiki/Anagrams

# [Anagrams][1]

```perl
my %anagram = slurp('unixdict.txt').words.classify( { .comb.sort.join } );
 
my $max = [max] map { +@($_) }, %anagram.values;
 
%anagram.values.grep( { +@($_) >= $max } )».join(' ')».say;
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


Just for the fun of it, here's one-liner that uses no temporaries.  Since it would be rather long, we've oriented it vertically:

```perl
 
.say for                              # print each element of the array made this way:
slurp('unixdict.txt')\                # load file in memory
.words\                               # extract words
.classify( *.comb.sort.join )\        # group by common anagram
.classify( *.value.elems )\           # group by number of anagrams in a group
.max( :by(*.key) ).value\             # get the group with highest number of anagrams
».value                               # get all groups of anagrams in the group just selected
```