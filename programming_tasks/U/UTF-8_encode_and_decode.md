[1]: https://rosettacode.org/wiki/UTF-8_encode_and_decode

# [UTF-8 encode and decode][1]

Pretty much all built in to the language.

```perl
say sprintf("%-18s %-36s|%8s| %7s |%14s | %s\n", 'Character|', 'Name', 'Ordinal', 'Unicode', 'UTF-8 encoded', 'decoded'), '-' x 100;
 
for < A ö Ж € 𝄞 😜 👨‍👩‍👧‍👦> -> $char {
    printf "   %-5s | %-43s | %6s | %-7s | %12s  |%4s\n", $char, $char.uninames.join(','), $char.ords.join(' '),
      ('U+' X~ $char.ords».base(16)).join(' '), $char.encode('UTF8').list».base(16).Str, $char.encode('UTF8').decode;
}
```

#### Output:
```
Character|         Name                                | Ordinal| Unicode | UTF-8 encoded | decoded
----------------------------------------------------------------------------------------------------
   A     | LATIN CAPITAL LETTER A                      |     65 | U+41    |           41  |   A
   ö     | LATIN SMALL LETTER O WITH DIAERESIS         |    246 | U+F6    |        C3 B6  |   ö
   Ж    | CYRILLIC CAPITAL LETTER ZHE                 |   1046 | U+416   |        D0 96  |   Ж
   €     | EURO SIGN                                   |   8364 | U+20AC  |     E2 82 AC  |   €
   𝄞     | MUSICAL SYMBOL G CLEF                       | 119070 | U+1D11E |  F0 9D 84 9E  |   𝄞
   😜    | FACE WITH STUCK-OUT TONGUE AND WINKING EYE  | 128540 | U+1F61C |  F0 9F 98 9C  |   😜
   👨‍👩‍👧‍👦    | MAN,ZERO WIDTH JOINER,WOMAN,ZERO WIDTH JOINER,GIRL,ZERO WIDTH JOINER,BOY | 128104 8205 128105 8205 128103 8205 128102 | U+1F468 U+200D U+1F469 U+200D U+1F467 U+200D U+1F466 | F0 9F 91 A8 E2 80 8D F0 9F 91 A9 E2 80 8D F0 9F 91 A7 E2 80 8D F0 9F 91 A6  |   👨‍👩‍👧‍👦
```