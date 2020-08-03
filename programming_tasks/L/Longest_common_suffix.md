[1]: https://rosettacode.org/wiki/Longest_common_suffix

# [Longest common suffix][1]

```raku
sub longest-common-suffix ( *@words ) {
    return '' unless +@words;
    my $min = @words».chars.min;
    for 1 .. * {
        return @words[0].substr(* - $min) if $_ > $min;
        next if @words».substr(* - $_).Set == 1;
        return @words[0].substr(* - $_ + 1);
    }
}

say "{$_.raku} - LCS: >{longest-common-suffix $_}<" for
  <Sunday Monday Tuesday Wednesday Thursday Friday Saturday>,
  <Sondag Maandag Dinsdag Woensdag Donderdag Vrydag Saterdag dag>,
  ( 2347, 9312347, 'acx5g2347', 12.02347 ),
  <longest common suffix>,
  ('one, Hey!', 'three, Hey!', 'ale, Hey!', 'me, Hey!'),
  'suffix',
  ''
```

#### Output:
```
("Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday") - LCS: >day<
("Sondag", "Maandag", "Dinsdag", "Woensdag", "Donderdag", "Vrydag", "Saterdag", "dag") - LCS: >dag<
(2347, 9312347, "acx5g2347", 12.02347) - LCS: >2347<
("longest", "common", "suffix") - LCS: ><
("one, Hey!", "three, Hey!", "ale, Hey!", "me, Hey!") - LCS: >e, Hey!<
"suffix" - LCS: >suffix<
"" - LCS: ><
```
