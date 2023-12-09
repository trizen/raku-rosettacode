[1]: https://rosettacode.org/wiki/Sphenic_numbers

# [Sphenic numbers][1]

Not the most efficient algorithm, but **massively** parallelizable, so finishes pretty quickly.

```perl
use Prime::Factor;
use List::Divvy;
use Lingua::EN::Numbers;

my @sphenic = lazy (^Inf).hyper(:200batch).grep: { my @pf = .&prime-factors; +@pf == 3 and +@pf.unique == 3 };
my @triplets = lazy (^Inf).grep( { @sphenic[$_]+2 == @sphenic[$_+2] } ).map: {(@sphenic[$_,$_+1,$_+2])}

say "Sphenic numbers less than 1,000:\n" ~
    @sphenic.&upto(1e3).batch(15)».fmt("%3d").join: "\n";

say "\nSphenic triplets less than 10,000:";
.say for @triplets.&before(*.[2] > 1e4);

say "\nThere are {(+@sphenic.&upto(1e6)).&comma} sphenic numbers less than {1e6.Int.&comma}";
say "There are {(+@triplets.&before(*.[2] > 1e6)).&comma} sphenic triplets less than {1e6.Int.&comma}";
say "The 200,000th sphenic number is {@sphenic[2e5-1].&comma} ({@sphenic[2e5-1].&prime-factors.join(' × ')}).";
say "The 5,000th sphenic triplet is ({@triplets[5e3-1].join(', ')})."
```

#### Output:
```
Sphenic numbers less than 1,000:
 30  42  66  70  78 102 105 110 114 130 138 154 165 170 174
182 186 190 195 222 230 231 238 246 255 258 266 273 282 285
286 290 310 318 322 345 354 357 366 370 374 385 399 402 406
410 418 426 429 430 434 435 438 442 455 465 470 474 483 494
498 506 518 530 534 555 561 574 582 590 595 598 602 606 609
610 615 618 627 638 642 645 646 651 654 658 663 665 670 678
682 705 710 715 730 741 742 754 759 762 777 782 786 790 795
805 806 814 822 826 830 834 854 861 874 885 890 894 897 902
903 906 915 935 938 942 946 957 962 969 970 978 986 987 994

Sphenic triplets less than 10,000:
(1309 1310 1311)
(1885 1886 1887)
(2013 2014 2015)
(2665 2666 2667)
(3729 3730 3731)
(5133 5134 5135)
(6061 6062 6063)
(6213 6214 6215)
(6305 6306 6307)
(6477 6478 6479)
(6853 6854 6855)
(6985 6986 6987)
(7257 7258 7259)
(7953 7954 7955)
(8393 8394 8395)
(8533 8534 8535)
(8785 8786 8787)
(9213 9214 9215)
(9453 9454 9455)
(9821 9822 9823)
(9877 9878 9879)

There are 206,964 sphenic numbers less than 1,000,000
There are 5,457 sphenic triplets less than 1,000,000
The 200,000th sphenic number is 966,467 (17 × 139 × 409).
The 5,000th sphenic triplet is (918005, 918006, 918007).
```
