[1]: http://rosettacode.org/wiki/Calculate_International_Securities_Identification_Number

# [Calculate International Securities Identification Number][1]

```perl
sub isin-checksum ( $isin --> Int ) {
    (10 - ([+] flat map {([+] .comb) % 10}, ([~] flat map {:36($_)}, $isin.comb).flip.comb «*» (2,1)) % 10) % 10
}
 
say "$_ -> {.&isin-checksum}" for
<
  US037833100
  US037383100
  SU037833100
  AU0000XVGZA
  AU0000VXGZA
  GB000263494
>
```

#### Output:
```
US037833100 -> 5
US037383100 -> 9
SU037833100 -> 5
AU0000XVGZA -> 3
AU0000VXGZA -> 3
GB000263494 -> 6
```