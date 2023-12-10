[1]: https://rosettacode.org/wiki/Smallest_number_k_such_that_k%2B2^m_is_composite_for_all_m_less_than_k

# [Smallest number k such that k+2^m is composite for all m less than k][1]

```perl
put (1..∞).hyper(:250batch).map(* × 2 + 1).grep( -> $k { !(1 ..^ $k).first: ($k + 1 +< *).is-prime } )[^5]
```

#### Output:
```
773 2131 2491 4471 5101
```
