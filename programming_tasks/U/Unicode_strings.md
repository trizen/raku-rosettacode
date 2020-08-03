[1]: https://rosettacode.org/wiki/Unicode_strings

# [Unicode strings][1]

Perl 6 programs and strings are all in Unicode and operate at a grapheme abstraction level, which is agnostic to underlying encodings or normalizations. (These are generally handled at program boundaries.) Opened files default to UTF-8 encoding. All Unicode character properties are in play, so any appropriate characters may be used as parts of identifiers, whitespace, or user-defined operators. For instance:

```perl
sub prefix:<∛> (\𝐕) { 𝐕 ** (1/3) }
say ∛27;  # prints 3
```


Non-Unicode strings are represented as Buf types rather than Str types, and Unicode operations may not be applied to Buf types without some kind of explicit conversion. Only ASCIIish operations are allowed on buffers.



As things develop, Perl 6 intends to support Unicode even better than Perl 5, which already does a great job in recent versions of accessing nearly all Unicode 6.0 functionality. Perl 6 improves on Perl 5 primarily by offering explicitly typed strings that always know which operations are sensical and which are not.