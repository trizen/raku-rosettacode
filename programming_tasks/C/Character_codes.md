[1]: https://rosettacode.org/wiki/Character_codes

# [Character codes][1]


Both Perl 5 and Raku have good Unicode support, though Raku attempts to make working with Unicode effortless.  Note that even multi-byte emoji and characters outside the BMP are considered single characters. Also note: all of these routines are built into the base compiler. No need to load external libraries. See [Wikipedia: Unicode character properties](https://en.wikipedia.org/wiki/Unicode_character_property#General_Category) for explanation of Unicode property.

```perl
for 'AΑА𪚥🇺🇸👨‍👩‍👧‍👦'.comb {
    .put for
    [ 'Character',
      'Character name',
      'Unicode property',
      'Unicode script',
      'Unicode block',
      'Added in Unicode version',
      'Ordinal(s)',
      'Hex ordinal(s)',
      'UTF-8',
      'UTF-16LE',
      'UTF-16BE',
      'Round trip by name',
      'Round trip by ordinal'
    ]».fmt('%25s:')
    Z
    [ $_,
      .uninames.join(', '),
      .uniprops.join(', '),
      .uniprops('Script').join(', '),
      .uniprops('Block').join(', '),
      .uniprops('Age').join(', '),
      .ords,
      .ords.fmt('0x%X'),
      .encode('utf8'   )».fmt('%02X'),
      .encode('utf16le')».fmt('%02X').join.comb(4),
      .encode('utf16be')».fmt('%02X').join.comb(4),
      .uninames».uniparse.join,
      .ords.chrs
    ];
    say '';
}
```

#### Output:
```
                Character: A
           Character name: LATIN CAPITAL LETTER A
         Unicode property: Lu
           Unicode script: Latin
            Unicode block: Basic Latin
 Added in Unicode version: 1.1
               Ordinal(s): 65
           Hex ordinal(s): 0x41
                    UTF-8: 41
                 UTF-16LE: 4100
                 UTF-16BE: 0041
       Round trip by name: A
    Round trip by ordinal: A

                Character: Α
           Character name: GREEK CAPITAL LETTER ALPHA
         Unicode property: Lu
           Unicode script: Greek
            Unicode block: Greek and Coptic
 Added in Unicode version: 1.1
               Ordinal(s): 913
           Hex ordinal(s): 0x391
                    UTF-8: CE 91
                 UTF-16LE: 9103
                 UTF-16BE: 0391
       Round trip by name: Α
    Round trip by ordinal: Α

                Character: А
           Character name: CYRILLIC CAPITAL LETTER A
         Unicode property: Lu
           Unicode script: Cyrillic
            Unicode block: Cyrillic
 Added in Unicode version: 1.1
               Ordinal(s): 1040
           Hex ordinal(s): 0x410
                    UTF-8: D0 90
                 UTF-16LE: 1004
                 UTF-16BE: 0410
       Round trip by name: А
    Round trip by ordinal: А

                Character: 𪚥
           Character name: CJK UNIFIED IDEOGRAPH-2A6A5
         Unicode property: Lo
           Unicode script: Han
            Unicode block: CJK Unified Ideographs Extension B
 Added in Unicode version: 3.1
               Ordinal(s): 173733
           Hex ordinal(s): 0x2A6A5
                    UTF-8: F0 AA 9A A5
                 UTF-16LE: 69D8 A5DE
                 UTF-16BE: D869 DEA5
       Round trip by name: 𪚥
    Round trip by ordinal: 𪚥

                Character: 🇺🇸
           Character name: REGIONAL INDICATOR SYMBOL LETTER U, REGIONAL INDICATOR SYMBOL LETTER S
         Unicode property: So, So
           Unicode script: Common, Common
            Unicode block: Enclosed Alphanumeric Supplement, Enclosed Alphanumeric Supplement
 Added in Unicode version: 6.0, 6.0
               Ordinal(s): 127482 127480
           Hex ordinal(s): 0x1F1FA 0x1F1F8
                    UTF-8: F0 9F 87 BA F0 9F 87 B8
                 UTF-16LE: 3CD8 FADD 3CD8 F8DD
                 UTF-16BE: D83C DDFA D83C DDF8
       Round trip by name: 🇺🇸
    Round trip by ordinal: 🇺🇸

                Character: 👨‍👩‍👧‍👦
           Character name: MAN, ZERO WIDTH JOINER, WOMAN, ZERO WIDTH JOINER, GIRL, ZERO WIDTH JOINER, BOY
         Unicode property: So, Cf, So, Cf, So, Cf, So
           Unicode script: Common, Inherited, Common, Inherited, Common, Inherited, Common
            Unicode block: Miscellaneous Symbols and Pictographs, General Punctuation, Miscellaneous Symbols and Pictographs, General Punctuation, Miscellaneous Symbols and Pictographs, General Punctuation, Miscellaneous Symbols and Pictographs
 Added in Unicode version: 6.0, 1.1, 6.0, 1.1, 6.0, 1.1, 6.0
               Ordinal(s): 128104 8205 128105 8205 128103 8205 128102
           Hex ordinal(s): 0x1F468 0x200D 0x1F469 0x200D 0x1F467 0x200D 0x1F466
                    UTF-8: F0 9F 91 A8 E2 80 8D F0 9F 91 A9 E2 80 8D F0 9F 91 A7 E2 80 8D F0 9F 91 A6
                 UTF-16LE: 3DD8 68DC 0D20 3DD8 69DC 0D20 3DD8 67DC 0D20 3DD8 66DC
                 UTF-16BE: D83D DC68 200D D83D DC69 200D D83D DC67 200D D83D DC66
       Round trip by name: 👨‍👩‍👧‍👦
    Round trip by ordinal: 👨‍👩‍👧‍👦
```
