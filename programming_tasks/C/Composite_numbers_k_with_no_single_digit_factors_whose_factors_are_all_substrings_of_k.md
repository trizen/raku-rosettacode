[1]: https://rosettacode.org/wiki/Composite_numbers_k_with_no_single_digit_factors_whose_factors_are_all_substrings_of_k

# [Composite numbers k with no single digit factors whose factors are all substrings of k][1]

```perl
use Prime::Factor;
use Lingua::EN::Numbers;
 
put (2..∞).hyper(:5000batch).map( {
    next if (1 < $_ gcd 210) || .is-prime || any .&prime-factors.map: -> $n { !.contains: $n };
    $_
} )[^20].batch(10)».&comma».fmt("%10s").join: "\n";
```

#### Output:
```
    15,317     59,177     83,731    119,911    183,347    192,413  1,819,231  2,111,317  2,237,411  3,129,361
 5,526,173 11,610,313 13,436,683 13,731,373 13,737,841 13,831,103 15,813,251 17,692,313 19,173,071 28,118,827
```
