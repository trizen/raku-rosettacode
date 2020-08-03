[1]: https://rosettacode.org/wiki/Soundex

# [Soundex][1]

US census algorithm, so "Ashcraft" and "Burroughs" adjusted to match.
We fake up a first consonant in some cases to make up for the fact that we always trim the first numeric code (so that the 'l' of 'Lloyd' is properly deleted).

```raku
sub soundex ($name --> Str) {
    my $first = substr($name,0,1).uc;
    gather {
        take $first;
        my $fakefirst = '';
        $fakefirst = "de " if $first ~~ /^ <[AEIOUWH]> /;
        "$fakefirst$name".lc.trans('wh' => '') ~~ /
            ^
            [
                [
                | <[ bfpv     ]>+ { take 1 }
                | <[ cgjkqsxz ]>+ { take 2 }
                | <[ dt       ]>+ { take 3 }
                | <[ l        ]>+ { take 4 }
                | <[ mn       ]>+ { take 5 }
                | <[ r        ]>+ { take 6 }
                ]
            || .
            ]+
            $ { take 0,0,0 }
        /;
    }.flat.[0,2,3,4].join;
}
 
for < Soundex     S532
      Example     E251
      Sownteks    S532
      Ekzampul    E251
      Euler       E460
      Gauss       G200
      Hilbert     H416
      Knuth       K530
      Lloyd       L300
      Lukasiewicz L222
      Ellery      E460
      Ghosh       G200
      Heilbronn   H416
      Kant        K530
      Ladd        L300
      Lissajous   L222
      Wheaton     W350
      Ashcraft    A261
      Burroughs   B620
      Burrows     B620
      O'Hara      O600 >
-> $n, $s {
    my $s2 = soundex($n);
    say $n.fmt("%16s "), $s, $s eq $s2 ?? " OK" !! " NOT OK $s2";
}
```

#### Output:
```
         Soundex S532 OK
         Example E251 OK
        Sownteks S532 OK
        Ekzampul E251 OK
           Euler E460 OK
           Gauss G200 OK
         Hilbert H416 OK
           Knuth K530 OK
           Lloyd L300 OK
     Lukasiewicz L222 OK
          Ellery E460 OK
           Ghosh G200 OK
       Heilbronn H416 OK
            Kant K530 OK
            Ladd L300 OK
       Lissajous L222 OK
         Wheaton W350 OK
        Ashcraft A261 OK
       Burroughs B620 OK
         Burrows B620 OK
          O'Hara O600 OK
```