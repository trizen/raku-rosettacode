[1]: http://rosettacode.org/wiki/Textonyms

# [Textonyms][1]

```perl
my $src = 'unixdict.txt';
 
my @words = slurp($src).lines.grep(/ ^ <alpha>+ $ /);
 
my @dials = @words.classify: {
    .trans('abcdefghijklmnopqrstuvwxyzABCDEFGHIJKLMNOPQRSTUVWXYZ'
        => '2223334445556667777888999922233344455566677778889999');
}
 
my @textonyms = @dials.grep(*.value > 1);
 
say qq:to 'END';
    There are {+@words} words in $src which can be represented by the digit key mapping.
    They require {+@dials} digit combinations to represent them.
    {+@textonyms} digit combinations represent Textonyms.
    END
 
say "Top 5 in ambiguity:";
say "    ",$_ for @textonyms.sort(-*.value)[^5];
 
say "\nTop 5 in length:";
say "    ",$_ for @textonyms.sort(-*.key.chars)[^5];
```

#### Output:
```
There are 24978 words in unixdict.txt which can be represented by the digit key mapping.
They require 22903 digit combinations to represent them.
1473 digit combinations represent Textonyms.

Top 5 in ambiguity:
    269 => amy any bmw bow box boy cow cox coy
    729 => paw pax pay paz raw ray saw sax say
    2273 => acre bard bare base cape card care case
    726 => pam pan ram ran sam san sao scm
    426 => gam gao ham han ian ibm ibn

Top 5 in length:
    25287876746242 => claustrophobia claustrophobic
    7244967473642 => schizophrenia schizophrenic
    666628676342 => onomatopoeia onomatopoeic
    49376746242 => hydrophobia hydrophobic
    2668368466 => contention convention
```