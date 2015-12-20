[1]: http://rosettacode.org/wiki/Idiomatically_determine_all_the_characters_that_can_be_used_for_symbols

# [Idiomatically determine all the characters that can be used for symbols][1]

Any Unicode character or combination of characters can be used for symbols in Perl 6. Here's some counting rods and some cuneiform:

```perl6
sub postfix:<𒋦>($n) { say "$n trilobites" }
 
sub term:<𝍧> { unival('𝍧') }
 
𝍧𒋦
```

#### Output:
```
8 trilobites
```


Of course, as in other languages, most of the characters you'll typically see in names are going to be alphanumerics from ASCII (or maybe Unicode), but that's a convention, not a limitation, due to the syntactic category notation demonstrated above, which can introduce any sequence of characters as a term or operator.



Actually, the above is a slight prevarication. The syntactic category notation does not allow you to use whitespace in the definition of a new symbol. But that leaves many more characters allowed than not allowed. Hence, it is much easier to enumerate the characters that <em>cannot</em> be used in symbols:

```perl6
say .fmt("%4x"),"\t", uniname($_)
    if uniprop($_,'Z')
        for 0..0x1ffff;
```

#### Output:
```
  20    SPACE
  a0    NO-BREAK SPACE
1680    OGHAM SPACE MARK
2000    EN QUAD
2001    EM QUAD
2002    EN SPACE
2003    EM SPACE
2004    THREE-PER-EM SPACE
2005    FOUR-PER-EM SPACE
2006    SIX-PER-EM SPACE
2007    FIGURE SPACE
2008    PUNCTUATION SPACE
2009    THIN SPACE
200a    HAIR SPACE
2028    LINE SEPARATOR
2029    PARAGRAPH SEPARATOR
202f    NARROW NO-BREAK SPACE
205f    MEDIUM MATHEMATICAL SPACE
3000    IDEOGRAPHIC SPACE
```


We enforce the whitespace restriction to prevent insanity in the readers of programs.
That being said, even the whitespace restriction is arbitrary, and can be bypassed by deriving a new grammar and switching to it. We view all other languages as dialects of Perl 6, even the insane ones. <tt>:-)</tt>