[1]: http://rosettacode.org/wiki/Entropy/Narcissist

# [Entropy/Narcissist][1]

```perl
say log(2) R/ [+] map -> \p { p * -log p }, .bag.values >>/>> +$_
    given slurp($*PROGRAM_NAME).comb
```

#### Output:
```
4.89351613053006
```