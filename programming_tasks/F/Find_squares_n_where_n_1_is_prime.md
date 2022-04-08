[1]: https://rosettacode.org/wiki/Find_squares_n_where_n%2B1_is_prime

# [Find squares n where n+1 is prime][1]

Use up to to one thousand (1,000) rather than up to one (1.000) as otherwise it would be a pretty short list...

```perl
say ({$++²}…^*>Ⅿ).grep: (*+1).is-prime
```

#### Output:
```
(1 4 16 36 100 196 256 400 576 676)
```


Although, technically, there is absolutely nothing in the task directions specifying that **n** needs to be the square of an *integer*. So, more accurately...

```perl
put (^Ⅿ).grep(*.is-prime).map(*-1).batch(20)».fmt("%3d").join: "\n"
```

#### Output:
```
  1   2   4   6  10  12  16  18  22  28  30  36  40  42  46  52  58  60  66  70
 72  78  82  88  96 100 102 106 108 112 126 130 136 138 148 150 156 162 166 172
178 180 190 192 196 198 210 222 226 228 232 238 240 250 256 262 268 270 276 280
282 292 306 310 312 316 330 336 346 348 352 358 366 372 378 382 388 396 400 408
418 420 430 432 438 442 448 456 460 462 466 478 486 490 498 502 508 520 522 540
546 556 562 568 570 576 586 592 598 600 606 612 616 618 630 640 642 646 652 658
660 672 676 682 690 700 708 718 726 732 738 742 750 756 760 768 772 786 796 808
810 820 822 826 828 838 852 856 858 862 876 880 882 886 906 910 918 928 936 940
946 952 966 970 976 982 990 996
```
