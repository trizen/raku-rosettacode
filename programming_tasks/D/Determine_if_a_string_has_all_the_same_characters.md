[1]: https://rosettacode.org/wiki/Determine_if_a_string_has_all_the_same_characters

# [Determine if a string has all the same characters][1]

The last string demonstrates how Perl 6 can recognize that glyphs made up of different combinations of combining characters can compare the same. It is built up from explicit codepoints to show that each of the glyphs is made up of different combinations.

```perl
  -> $str {
    my $i = 0;
    print "\n{$str.perl} (length: {$str.chars}), has ";
    my %m;
    %m{$_}.push: ++$i for $str.comb;
 
    if %m > 1 {
        say "different characters:";
        say "'{.key}' ({.key.uninames}; hex ordinal: {(.key.ords).fmt: "0x%X"})" ~
        " in positions: {.value.join: ', '}" for %m.sort( *.value[0] );
    } else {
        say "the same character in all positions."
    }
} for
    '',
    '   ',
    '2',
    '333',
    '.55',
    'tttTTT',
    '4444 444k',
    '🇬🇧🇬🇧🇬🇧🇬🇧',
    "\c[LATIN CAPITAL LETTER A]\c[COMBINING DIAERESIS]\c[COMBINING MACRON]" ~
    "\c[LATIN CAPITAL LETTER A WITH DIAERESIS]\c[COMBINING MACRON]" ~
    "\c[LATIN CAPITAL LETTER A WITH DIAERESIS AND MACRON]",
    'AАΑꓮ𐌀𐊠Ꭺ'
```

#### Output:
```
"" (length: 0), has the same character in all positions.

"   " (length: 3), has the same character in all positions.

"2" (length: 1), has the same character in all positions.

"333" (length: 3), has the same character in all positions.

".55" (length: 3), has different characters:
'.' (FULL STOP; hex ordinal: 0x2E) in positions: 1
'5' (DIGIT FIVE; hex ordinal: 0x35) in positions: 2, 3

"tttTTT" (length: 6), has different characters:
't' (LATIN SMALL LETTER T; hex ordinal: 0x74) in positions: 1, 2, 3
'T' (LATIN CAPITAL LETTER T; hex ordinal: 0x54) in positions: 4, 5, 6

"4444 444k" (length: 9), has different characters:
'4' (DIGIT FOUR; hex ordinal: 0x34) in positions: 1, 2, 3, 4, 6, 7, 8
' ' (SPACE; hex ordinal: 0x20) in positions: 5
'k' (LATIN SMALL LETTER K; hex ordinal: 0x6B) in positions: 9

"🇬🇧🇬🇧🇬🇧🇬🇧" (length: 4), has the same character in all positions.

"ǞǞǞ" (length: 3), has the same character in all positions.

"AАΑꓮ𐌀𐊠Ꭺ" (length: 7), has different characters:
'A' (LATIN CAPITAL LETTER A; hex ordinal: 0x41) in positions: 1
'А' (CYRILLIC CAPITAL LETTER A; hex ordinal: 0x410) in positions: 2
'Α' (GREEK CAPITAL LETTER ALPHA; hex ordinal: 0x391) in positions: 3
'ꓮ' (LISU LETTER A; hex ordinal: 0xA4EE) in positions: 4
'𐌀' (OLD ITALIC LETTER A; hex ordinal: 0x10300) in positions: 5
'𐊠' (CARIAN LETTER A; hex ordinal: 0x102A0) in positions: 6
'Ꭺ' (CHEROKEE LETTER GO; hex ordinal: 0x13AA) in positions: 7
```
