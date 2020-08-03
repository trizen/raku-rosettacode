[1]: https://rosettacode.org/wiki/Strip_a_set_of_characters_from_a_string

# [Strip a set of characters from a string][1]

```raku
sub strip_chars ( $s, $chars ) {
    return $s.trans( $chars.comb X=> '' );
}
 
say strip_chars( 'She was a soul stripper. She took my heart!', 'aei' );
```

#### Output:
```
Sh ws  soul strppr. Sh took my hrt!
```