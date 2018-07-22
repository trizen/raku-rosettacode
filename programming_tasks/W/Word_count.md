[1]: https://rosettacode.org/wiki/Word_count

# [Word count][1]

Note: much of the following exposition is no longer critical to the task as the requirements have been updated, but is left here for historical and informational reasons.



This is slightly trickier than it appears initially. The task specifically states: "A word is a sequence of one or more contiguous letters", so contractions and hyphenated words are broken up. Initially we might reach for a regex matcher like /\w+/ , but \w includes underscore, which is not a letter but a punctuation connector; and this text is **full** of underscores since that is how Project Gutenberg texts denote italicized text. The underscores are not actually parts of the words though, they are markup.



We might try /A-Za-z/ as a matcher but this text is bursting with French words containing various accented glyphs. Those **are** letters, so words will be incorrectly split up; (Misérables will be counted as 'mis' and 'rables', probably not what we want.)



Actually, in this case /A-Za-z/ returns **very nearly** the correct answer. Unfortunately, the name "Alèthe" appears once (only once!) in the text, gets incorrectly split into Al &amp; the, and incorrectly reports 41089 occurrences of "the".
The text has several words like "Panathenæa", "ça", "aérostiers" and "Keksekça" so the counts for 'a' are off too. The other 8 of the top 10 are "correct" using /A-Za-z/, but it is mostly by accident.



A more accurate regex matcher would be some kind of Unicode aware /\w/ minus underscore. It may also be useful, depending on your requirements, to recognize contractions with embedded apostrophes, hyphenated words, and hyphenated words broken across lines.



Here is a sample that shows the result when using various different matchers.

```perl
sub MAIN ($filename, $top = 10) {
    my $file = $filename.IO.slurp.lc.subst(/ (<[\w]-[_]>'-')\n(<[\w]-[_]>) /, {$0 ~ $1}, :g );
    my @matcher = (
        rx/ <[a..z]>+ /,    # simple 7-bit ASCII
        rx/ \w+ /,          # word characters with underscore
        rx/ <[\w]-[_]>+ /,  # word characters without underscore
        rx/ <[\w]-[_]>+[["'"|'-'|"'-"]<[\w]-[_]>+]* /   # word characters without underscore but with hyphens and contractions
    );
    for @matcher -> $reg {
        say "\nTop $top using regex: ", $reg.perl;
        .put for $file.comb( $reg ).Bag.sort(-*.value)[^$top];
    }
}
```


Passing in the file name and 10:


#### Output:
```
Top 10 using regex: rx/ <[a..z]>+ /
the     41089
of      19949
and     14942
a       14608
to      13951
in      11214
he      9648
was     8621
that    7924
it      6661

Top 10 using regex: rx/ \w+ /
the     41035
of      19946
and     14940
a       14577
to      13939
in      11204
he      9645
was     8619
that    7922
it      6659

Top 10 using regex: rx/ <[\w]-[_]>+ /
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

Top 10 using regex: rx/ <[\w]-[_]>+[["'"|'-'|"'-"]<[\w]-[_]>+]* /
the     41081
of      19930
and     14934
a       14587
to      13735
in      11204
he      9607
was     8620
that    7825
it      6535
```