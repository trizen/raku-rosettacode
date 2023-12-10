[1]: https://rosettacode.org/wiki/Find_words_which_contains_more_than_3_e_vowels

# [Find words which contains more than 3 e vowels][1]

Hardly even worth saving as a script file; an easily entered one-liner.

```perl
.say for "unixdict.txt".IO.lines.grep: { !/<[aiou]>/ and /e.*e.*e.*e/ };
```

#### Output:
```
belvedere
dereference
elsewhere
erlenmeyer
evergreen
everywhere
exegete
freewheel
nevertheless
persevere
preference
referee
seventeen
seventeenth
telemeter
tennessee
```


In an attempt to be a little less useless, here's an alternate script that allows you to specify a vowel, the minimum count to search for, and the file to search. Counts base vowels case, and combining accent insensitive; works with *any* text file, not just word lists. Defaults to finding words with at least 4 'e's in unixdict.txt. Default output is same as above.

```perl
unit sub MAIN ($vowel = 'e', $min = 4, $file = 'unixdict.txt');
.say for squish sort
  ( $file.IO.slurp.words.grep: { ((my $b = .lc.samemark(' ').comb.Bag){$vowel} >= $min) && $b<a e i o u>.sum == $b{$vowel} } )\
  ».subst(/<[":;,.?!_\[\]]>/, '', :g);
```


How about: find words with at least 4 'a's in the [Les Misérables file](https://github.com/thundergnat/rc-run/blob/master/rc/resources/lemiz.txt) used for the [Word frequency](https://rosettacode.org/wiki/Word_frequency) task?



Command line `> raku monovowel a 4 lemiz.txt`


```
Caracalla
Caracara
Salamanca
Vâsaphantâ
```
