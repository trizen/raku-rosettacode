[1]: https://rosettacode.org/wiki/Unicode_strings

# [Unicode strings][1]





Raku programs and strings are all in Unicode and operate at a grapheme abstraction level, which is agnostic to underlying encodings.  (These are generally handled at program boundaries.)  Opened files default to UTF-8 encoding. All Unicode character properties are in play, so any appropriate characters may be used as parts of identifiers, whitespace, or user-defined operators.  For instance:

```perl
sub prefix:<∛> (\𝐕) { 𝐕 ** ⅓ }
say ∛27;  # prints 3
```


Non-Unicode strings are represented as Buf types rather than Str types, and Unicode operations may not be applied to Buf types without some kind of explicit conversion.  Only ASCIIish operations are allowed on buffers.



Raku tracks the Unicode consortium standards releases and is generally up to the latest standard within a few months or so of its release. (currently at 15.0 as of February 2023)



In general, it tries to make dealing with Unicode "just work".



Raku intends to support Unicode even better than Perl 5, which already does a great job in recent versions of accessing large swaths of Unicode spec. functionality. Raku improves on Perl 5 primarily by offering explicitly typed strings that always know which operations are sensical and which are not.



A very important distinctive characteristic of Raku to keep in mind is that it applies normalization (Unicode NFC form (Normalization Form Canonical)) automatically by default to all strings as showcased and explained on the [String comparison page](https://rosettacode.org/wiki/String_comparison#Unicode_normalization_by_default).
