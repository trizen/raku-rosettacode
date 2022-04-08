[1]: https://rosettacode.org/wiki/Determine_sentence_type

# [Determine sentence type][1]

```perl
use Lingua::EN::Sentence;
 
my $paragraph = q:to/PARAGRAPH/;
hi there, how are you today? I'd like to present to you the washing machine
9001. You have been nominated to win one of these! Just make sure you don't
break it
 
 
Just because there are punctuation characters like "?", "!" or especially "."
present, it doesn't necessarily mean you have reached the end of a sentence,
does it Mr. Magoo? The syntax highlighting here for Raku isn't the best.
PARAGRAPH
 
say join "\n\n", $paragraph.&get_sentences.map: {
    /(<:punct>)$/;
    $_ ~ ' | ' ~ do
    given $0 {
        when '!' { 'E' };
        when '?' { 'Q' };
        when '.' { 'S' };
        default  { 'N' };
    }
}
```

#### Output:
```
hi there, how are you today? | Q

I'd like to present to you the washing machine
9001. | S

You have been nominated to win one of these! | E

Just make sure you don't
break it | N

Just because there are punctuation characters like "?", "!" or especially "."
present, it doesn't necessarily mean you have reached the end of a sentence,
does it Mr. Magoo? | Q

The syntax highlighting here for Raku isn't the best. | S
```
