[1]: https://rosettacode.org/wiki/Entropy/Narcissist

# [Entropy/Narcissist][1]



```perl
say log(2) R/ [+] map -> \p { p * -log p }, $_.comb.Bag.values >>/>> +$_
    given slurp($*PROGRAM-NAME).comb
```


Result should be in the neighborhood of 4.9


```
4.89351613053006
```
