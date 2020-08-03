[1]: https://rosettacode.org/wiki/Determine_if_a_string_has_all_unique_characters

# [Determine if a string has all unique characters][1]

Perl 6 works with unicode natively and handles combining characters and multi-byte emoji correctly. In the last string, notice the the length is correctly shown as 11 characters and that the delta with a combining circumflex in position 6 is not the same as the deltas without in positions 5 &amp; 9.

```raku
  -> $str {
    my $i = 0;
    print "\n{$str.perl} (length: {$str.chars}), has ";
    my %m;
    %m{$_}.push: ++$i for $str.comb;
    if any(%m.values) > 1 {
        say "duplicated characters:";
        say "'{.key}' ({.key.uninames}; hex ordinal: {(.key.ords).fmt: "0x%X"})" ~
        " in positions: {.value.join: ', '}" for %m.grep( *.value > 1 ).sort( *.value[0] );
    } else {
        say "no duplicated characters."
    }
} for
    '',
    '.',
    'abcABC',
    'XYZ ZYX',
    '1234567890ABCDEFGHIJKLMN0PQRSTUVWXYZ',
    '01234567890ABCDEFGHIJKLMN0PQRSTUVWXYZ0X',
    '🦋🙂👨‍👩‍👧‍👦🙄ΔΔ̂ 🦋Δ👍👨‍👩‍👧‍👦'
```

#### Output:
```
"" (length: 0), has no duplicated characters.

"." (length: 1), has no duplicated characters.

"abcABC" (length: 6), has no duplicated characters.

"XYZ ZYX" (length: 7), has duplicated characters:
'X' (LATIN CAPITAL LETTER X; hex ordinal: 0x58) in positions: 1, 7
'Y' (LATIN CAPITAL LETTER Y; hex ordinal: 0x59) in positions: 2, 6
'Z' (LATIN CAPITAL LETTER Z; hex ordinal: 0x5A) in positions: 3, 5

"1234567890ABCDEFGHIJKLMN0PQRSTUVWXYZ" (length: 36), has duplicated characters:
'0' (DIGIT ZERO; hex ordinal: 0x30) in positions: 10, 25

"01234567890ABCDEFGHIJKLMN0PQRSTUVWXYZ0X" (length: 39), has duplicated characters:
'0' (DIGIT ZERO; hex ordinal: 0x30) in positions: 1, 11, 26, 38
'X' (LATIN CAPITAL LETTER X; hex ordinal: 0x58) in positions: 35, 39

"🦋🙂👨‍👩‍👧‍👦🙄ΔΔ̂ 🦋Δ👍👨‍👩‍👧‍👦" (length: 11), has duplicated characters:
'🦋' (BUTTERFLY; hex ordinal: 0x1F98B) in positions: 1, 8
'👨‍👩‍👧‍👦' (MAN ZERO WIDTH JOINER WOMAN ZERO WIDTH JOINER GIRL ZERO WIDTH JOINER BOY; hex ordinal: 0x1F468 0x200D 0x1F469 0x200D 0x1F467 0x200D 0x1F466) in positions: 3, 11
'Δ' (GREEK CAPITAL LETTER DELTA; hex ordinal: 0x394) in positions: 5, 9
```
