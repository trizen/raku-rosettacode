[1]: https://rosettacode.org/wiki/Word_frequency

# [Word frequency][1]





Note: much of the following exposition is no longer critical to the task as the requirements have been updated, but is left here for historical and informational reasons.



This is slightly trickier than it appears initially. The task specifically states: "A word is a sequence of one or more contiguous letters", so contractions and hyphenated words are broken up. Initially we might reach for a regex matcher like /\w+/ , but \w includes underscore, which is not a letter but a punctuation connector; and this text is **full** of underscores since that is how Project Gutenberg texts denote italicized text. The underscores are not actually parts of the words though, they are markup.



We might try /A-Za-z/ as a matcher but this text is bursting with French words containing various [diacritics](https://en.wikipedia.org/wiki/diacritic). Those **are** letters, so words will be incorrectly split up; (Misérables will be counted as 'mis' and 'rables', probably not what we want.)



Actually, in this case /A-Za-z/ returns **very nearly** the correct answer. Unfortunately, the name "Alèthe" appears once (only once!) in the text, gets incorrectly split into Al &amp; the, and incorrectly reports 41089 occurrences of "the".
The text has several words like "Panathenæa", "ça", "aérostiers" and "Keksekça" so the counts for 'a' are off too. The other 8 of the top 10 are "correct" using /A-Za-z/, but it is mostly by accident.



A more accurate regex matcher would be some kind of Unicode aware /\w/ minus underscore. It may also be useful, depending on your requirements, to recognize contractions with embedded apostrophes, hyphenated words, and hyphenated words broken across lines.



Here is a  sample that shows the result when using various different matchers.

```perl
sub MAIN ($filename, UInt $top = 10) {
    my $file = $filename.IO.slurp.lc.subst(/ (<[\w]-[_]>'-')\n(<[\w]-[_]>) /, {$0 ~ $1}, :g );
    my @matcher = 
        rx/ <[a..z]>+ /,    # simple 7-bit ASCII
        rx/ \w+ /,          # word characters with underscore
        rx/ <[\w]-[_]>+ /,  # word characters without underscore
        rx/ [<[\w]-[_]>+]+ % < ' - '- > /  # word characters without underscore but with hyphens and contractions
    ;
    for @matcher -> $reg {
        say "\nTop $top using regex: ", $reg.raku;
	    my @words = $file.comb($reg).Bag.sort(-*.value)[^$top];
	    my $length = max @words».key».chars;
        printf "%-{$length}s %d\n", .key, .value for @words;
    }
}
```


Passing in the file name and 10:


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


It can be difficult to figure out what words the different regexes do or don't match. Here are the three more complex regexes along with a list of "words" that are treated as being different using this regex as opposed to /a..z/. IE: It is lumped in as one of the top 10 word counts using /a..z/ but not with this regex.


```
Top 10 using regex: rx/ \w+ /
the     41035   alèthe _the _the_
of      19946   of_ _of_
and     14940   _and_ paternoster_and
a       14577   _ça aïe ça keksekça aérostiers _a poréa panathenæa
to      13939   to_ _to
in      11204   _in
he      9645    _he
was     8619    _was
that    7922    _that
it      6659    _it

Top 10 using regex: rx/ <[\w]-[_]>+ /
the     41088   alèthe
of      19949   
and     14942   
a       14596   poréa ça aérostiers panathenæa aïe keksekça
to      13951   
in      11214   
he      9648    
was     8621    
that    7924    
it      6661    

Top 10 using regex: rx/ <[\w]-[_]>+[["'"|'-'|"'-"]<[\w]-[_]>+]* /
the     41081   will-o'-the-wisps alèthe skip-the-gutter police-agent-ja-vert-was-found-drowned-un-der-a-boat-of-the-pont-au-change jean-the-screw will-o'-the-wisp
of      19930   chromate-of-lead-colored die-of-hunger die-of-cold-if-you-have-bread police-agent-ja-vert-was-found-drowned-un-der-a-boat-of-the-pont-au-change unheard-of die-of-hunger-if-you-have-a-fire
and     14934   come-and-see so-and-so cock-and-bull hide-and-seek sambre-and-meuse
a       14587   keksekça l'a ça now-a-days vis-a-vis a-dreaming police-agent-ja-vert-was-found-drowned-un-der-a-boat-of-the-pont-au-change poréa panathenæa aérostiers a-hunting aïe die-of-hunger-if-you-have-a-fire
to      13735   to-morrow to-day hand-to-hand to-night well-to-do face-to-face
in      11204   in-pace son-in-law father-in-law whippers-in general-in-chief sons-in-law
he      9607    he's he'll
was     8620    police-agent-ja-vert-was-found-drowned-un-der-a-boat-of-the-pont-au-change
that    7825    that's pick-me-down-that
it      6535    it's it'll
```


One nice thing is this isn't special cased. It will work out of the box for any text / language.



[Russian](https://www.gutenberg.org/files/14741/14741-0.txt)? No problem.


```
$ raku wf 14741-0.txt 5
```

```
Top 5 using regex: rx/ <[a..z]>+ /
the     176
of      119
gutenberg       93
project 87
to      80

Top 5 using regex: rx/ \w+ /
и       860
в       579
не      290
на      222
ты      195

Top 5 using regex: rx/ <[\w]-[_]>+ /
и       860
в       579
не      290
на      222
ты      195

Top 5 using regex: rx/ <[\w]-[_]>+[["'"|'-'|"'-"]<[\w]-[_]>+]* /
и       860
в       579
не      290
на      222
ты      195
```


[Greek](https://www.gutenberg.org/files/39963/39963-0.txt)? Sure, why not.


```
$ raku wf 39963-0.txt 5
```

```
Top 5 using regex: rx/ <[a..z]>+ /
the     187
of      123
gutenberg       93
project 87
to      82

Top 5 using regex: rx/ \w+ /
και     1628
εις     986
δε      982
του     895
των     859

Top 5 using regex: rx/ <[\w]-[_]>+ /
και     1628
εις     986
δε      982
του     895
των     859

Top 5 using regex: rx/ <[\w]-[_]>+[["'"|'-'|"'-"]<[\w]-[_]>+]* /
και     1628
εις     986
δε      982
του     895
των     859
```


Of course, for the first matcher, we are asking specifically to match Latin ASCII, so we end up with... well... Latin ASCII; but the other 3 match any Unicode characters.
