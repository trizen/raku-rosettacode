[1]: http://rosettacode.org/wiki/Word_count

# [Word count][1]

This is slightly trickier than it appears initially. The task specifically states: "A word is a sequence of one or more contiguous letters", so contractions and hyphenated words are broken up. Initially we might reach for a regex matcher like /\w+/ , but \w includes underscore, which is not a letter but a punctuation connector; and this text is **full** of underscores since that is how Project Gutenberg texts denote italicized text. The underscores are not actually parts of the words though, they are markup.



We might try /A-Za-z/ as a matcher but this text is bursting with French words containing various accented glyphs. Those **are** letters, so words will be incorrectly split up; (Misérables will be counted as 'mis' and 'rables', probably not what we want.)



Actually, in this case /A-Za-z/ returns **very nearly** the correct answer. Unfortunately, the name "Alèthe" appears once (only once!) in the text, gets incorrectly split into Al &amp; the, and incorrectly reports 41089 occurrences of "the".
The text has several words like "Panathenæa", "ça", "aérostiers" and "Keksekça" so the counts for 'a' are off too. The other 8 of the top 10 are "correct" using /A-Za-z/, but it is mostly by accident. The more accurate regex matcher is some kind of Unicode aware /\w/ minus underscore.



( Really, a better regex would allow for contractions and embedded apostrophes but that is beyond the scope of this task as it stands. There are words like cat-o'-nine-tails and will-o'-the-wisps in there too to make your day even more interesting. )

```perl
sub MAIN ($filename, $top = 10) {
    .put for $filename.IO.slurp.lc.comb( /[<[\w]-[_]>]+/ ).Bag.sort(-*.value)[^$top]
}
```


Passing in the file name and 10:


#### Output:
```
the     41088
of      19949
and     14942
a       14596
to      13951
in      11214
he      9648
was     8621
that    7924
it      6661
```


Or, as a one-liner at the command prompt:



`perl6 -e'lines.lc.comb( /[&lt;[\w]-[_]&gt;]+/ ).Bag.sort(-*.value)[^10].join("\n").say' &lt; ./lemiz.txt`



Same output.



This satisfies the task requirements as they are written, but leaves a lot to be desired. For my own amusement here is a version that recognizes contractions with embedded apostrophes, hyphenated words, and hyphenated words broken across lines. Returns the top N words and counts sorted by length with a secondary sort on frequency just to be different (and to demonstrate that it really does what is claimed.)

```perl
sub MAIN ($filename, $top = 10) {
    .put for $filename.IO.slurp.lc.subst(/ (<[\w]-[_]>'-')\n(<[\w]-[_]>) /, {$0 ~ $1}, :g )\
    .comb( / <[\w]-[_]>+[["'"|'-'|"'-"]<[\w]-[_]>+]* / ).Bag.sort( {-$^a.key.chars, -$a.value} )[^$top];
}
```


Again, passing in the same file name and 10:


#### Output:
```
police-agent-ja-vert-was-found-drowned-un-der-a-boat-of-the-pont-au-change      1
jésus-mon-dieu-bancroche-à-bas-la-lune  1
die-of-hunger-if-you-have-a-fire        1
guimard-guimardini-guimardinette        1
monsieur-i-don't-know-your-name 1
sainte-croix-de-la-bretonnerie  2
die-of-cold-if-you-have-bread   1
petit-picpus-sainte-antoine     1
saint-jacques-du-haut-pas       7
chemin-vert-saint-antoine       3
```