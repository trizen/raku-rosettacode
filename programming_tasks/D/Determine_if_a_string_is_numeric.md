[1]: https://rosettacode.org/wiki/Determine_if_a_string_is_numeric

# [Determine if a string is numeric][1]

Perl 6 tries very hard to DWIM (do what I mean). As part of that, numeric strings are by default stored as allomorphic types which can be used as numbers or strings without any conversion. If we truly want to operate on strings, we have to explicitly coerce the allomorphs to strings. A subtlety that may not be immediately apparent, whitespace, empty strings and null strings may be treated as (False) boolean values in Perl 6, however booleans are allomorphic to numeric, so empty strings will coerce to a numeric value (0), and return as numeric unless specifically checked for.



Note: These routines are usable for most cases but won't detect unicode non-digit numeric forms; E.G. vulgar fractions, Roman numerals, circled numbers, etc. If it is necessary to detect those as numeric, a full fledged grammar may be necessary.

```perl
sub is-number-w-ws( Str $term --> Bool ) { # treat Falsey strings as numeric
    $term.Numeric !~~ Failure;
}
 
sub is-number-wo-ws( Str $term --> Bool ) { # treat Falsey strings as non-numeric
    ?($term ~~ / \S /) && $term.Numeric !~~ Failure;
}
 
say "               Coerce     Don't coerce";
say '    String   whitespace    whitespace';
printf "%10s  %8s  %11s\n",
"<$_>", .&is-number-w-ws, .&is-number-wo-ws for
(|<1 1.2 1.2.3 -6 1/2 12e B17 1.3e+12 1.3e12 -2.6e-3 zero
0x 0xA10 0b1001 0o16 0o18 2+5i>, '1 1 1', '', ' ').map: *.Str;
```

#### Output:
```
               Coerce     Don't coerce
    String   whitespace    whitespace
       <1>      True         True
     <1.2>      True         True
   <1.2.3>     False        False
      <-6>      True         True
     <1/2>      True         True
     <12e>     False        False
     <B17>     False        False
 <1.3e+12>      True         True
  <1.3e12>      True         True
 <-2.6e-3>      True         True
    <zero>     False        False
      <0x>     False        False
   <0xA10>      True         True
  <0b1001>      True         True
    <0o16>      True         True
    <0o18>     False        False
    <2+5i>      True         True
   <1 1 1>     False        False
        <>      True        False
       < >      True        False
```