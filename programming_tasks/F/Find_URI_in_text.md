[1]: https://rosettacode.org/wiki/Find_URI_in_text

# [Find URI in text][1]



```perl
use v6;
use IETF::RFC_Grammar::URI;

say q:to/EOF/.match(/ <IETF::RFC_Grammar::URI::absolute-URI> /, :g).list.join("\n");
    this URI contains an illegal character, parentheses and a misplaced full stop:
    https://en.wikipedia.org/wiki/Erich_Kästner_(camera_designer). (which is handled by http://mediawiki.org/).
    and another one just to confuse the parser: https://en.wikipedia.org/wiki/-)
    ")" is handled the wrong way by the mediawiki parser.
    ftp://domain.name/path(balanced_brackets)/foo.html
    ftp://domain.name/path(balanced_brackets)/ending.in.dot.
    ftp://domain.name/path(unbalanced_brackets/ending.in.dot.
    leading junk ftp://domain.name/path/embedded?punct/uation.
    leading junk ftp://domain.name/dangling_close_paren)
    EOF

say $/[*-1];
say "We matched $/[*-1], which is a $/[*-1].^name() at position $/[*-1].from() to $/[*-1].to()"
```


Like most of the solutions here it does not comply to IRI but only to URI:


```
stop:
https://en.wikipedia.org/wiki/Erich_K
http://mediawiki.org/).
parser:
https://en.wikipedia.org/wiki/-)
ftp://domain.name/path(balanced_brackets)/foo.html
ftp://domain.name/path(balanced_brackets)/ending.in.dot.
ftp://domain.name/path(unbalanced_brackets/ending.in.dot.
ftp://domain.name/path/embedded
ftp://domain.name/dangling_close_paren)
｢ftp://domain.name/dangling_close_paren)｣
 IETF::RFC_Grammar::URI::absolute_URI => ｢ftp://domain.name/dangling_close_paren)｣
  scheme => ｢ftp｣
We matched ftp://domain.name/dangling_close_paren), which is a Match, at position 554 to 593
```


The last lines show that we get Match objects back that we can query to get all kinds of information.
We even get the information what subrules matched, and since these are also Match objects we can obtain
their match position in the text.
