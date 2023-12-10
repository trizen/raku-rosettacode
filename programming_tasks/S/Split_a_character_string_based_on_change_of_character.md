[1]: https://rosettacode.org/wiki/Split_a_character_string_based_on_change_of_character

# [Split a character string based on change of character][1]



```perl
sub group-chars ($str) { $str.comb: / (.) $0* / }

# Testing:

for Q[gHHH5YY++///\], Q[fff﻿﻿﻿n⃗n⃗n⃗»»»  ℵℵ☄☄☃☃̂☃🤔🇺🇸🤦‍♂️👨‍👩‍👧‍👦] -> $string {
    put 'Original: ', $string;
    put '   Split: ', group-chars($string).join(', ');
}
```

#### Output:
```
Original: gHHH5YY++///\
   Split: g, HHH, 5, YY, ++, ///, \
Original: fff﻿﻿﻿n⃗n⃗n⃗»»»  ℵℵ☄☄☃☃̂☃🤔🇺🇸🤦‍♂️👨‍👩‍👧‍👦
   Split: fff, ﻿﻿﻿, n⃗n⃗n⃗, »»»,   , ℵℵ, ☄☄, ☃, ☃̂, ☃, 🤔, 🇺🇸, 🤦‍♂️, 👨‍👩‍👧‍👦
```


The second test-case is to show that Raku works with strings on the Unicode grapheme level, handles whitespace, combiners, and zero width characters up to Unicode Version 13.0 correctly. (Raku generally tracks updates to the Unicode spec and typically lags no more than a month behind.) For those of you with browsers unable to display the second string, it consists of:
