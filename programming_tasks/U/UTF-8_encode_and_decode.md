[1]: http://rosettacode.org/wiki/UTF-8_encode_and_decode

# [UTF-8 encode and decode][1]

Pretty much all built in to the language.

```perl
say sprintf("%-18s %-34s %7s %7s\t%s  %s\n", 'Character', 'Name', 'Ordinal', 'Unicode', 'UTF-8 encoded', 'decoded'), '-' x 94;
 
for < A ö Ж € 𝄞 😜 > -> $char {
    printf "   %-7s %-43s %6s U+%04s\t%12s %4s\n", $char, $char.uniname, $char.ord,
      $char.ord.base(16), $char.encode('UTF8').list».base(16).Str, $char.encode('UTF8').decode;
}
```

#### Output:
```
Character          Name                               Ordinal Unicode   UTF-8 encoded  decoded
----------------------------------------------------------------------------------------------
   A       LATIN CAPITAL LETTER A                          65 U+0041              41    A
   ö       LATIN SMALL LETTER O WITH DIAERESIS            246 U+00F6           C3 B6    ö
   Ж       CYRILLIC CAPITAL LETTER ZHE                   1046 U+0416           D0 96    Ж
   €       EURO SIGN                                     8364 U+20AC        E2 82 AC    €
   𝄞       MUSICAL SYMBOL G CLEF                       119070 U+1D11E    F0 9D 84 9E    𝄞
   😜      FACE WITH STUCK-OUT TONGUE AND WINKING EYE  128540 U+1F61C     F0 9F 98 9C    😜
```