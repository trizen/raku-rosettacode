[1]: https://rosettacode.org/wiki/Extra_primes

# [Extra primes][1]

For the time being, (Doctor?), I'm going to assume that the task is really "Sequence of primes with every digit a prime and the sum of the digits a prime". Outputting my own take on a reasonable display of results, compact and easily doable but exercising it a bit.

```perl
my @ppp = lazy flat 2, 3, 5, 7, 23, grep { .is-prime && .comb.sum.is-prime },
               flat (2..*).map: { flat ([X~] (2, 3, 5, 7) xx $_) X~ (3, 7) };

put 'Terms < 10,000: '.fmt('%34s'), @ppp[^(@ppp.first: * > 1e4, :k)];
put '991st through 1000th: '.fmt('%34s'), @ppp[990 .. 999];
put 'Crossing 10th order of magnitude: ', @ppp[9055..9060];
```

#### Output:
```
                  Terms < 10,000: 2 3 5 7 23 223 227 337 353 373 557 577 733 757 773 2333 2357 2377 2557 2753 2777 3253 3257 3323 3527 3727 5233 5237 5273 5323 5527 7237 7253 7523 7723 7727
            991st through 1000th: 25337353 25353227 25353373 25353577 25355227 25355333 25355377 25357333 25357357 25357757
Crossing 10th order of magnitude: 777777227 777777577 777777773 2222222377 2222222573 2222225273
```
