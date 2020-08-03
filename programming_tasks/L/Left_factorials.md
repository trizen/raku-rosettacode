[1]: https://rosettacode.org/wiki/Left_factorials

# [Left factorials][1]

Perl 6 doesn't have a built in factorial function, so the first two lines implement postfix&#160;! factorial.
The newly implemented factorial function is used to implement left factorial using a prefix&#160;! in the next two lines.
Note that this redefines the core prefix&#160;! (not) function.
The last two lines are display code for the various sub task requirements.

```raku
multi sub postfix:<!> (0) { 1 };
multi sub postfix:<!> ($n) { [*] 1 .. $n };
multi sub prefix:<!> (0) { 0 };
multi sub prefix:<!> ($k) { [+] (^$k).map: { $_! } }
 
printf "!%d  = %s\n", $_, !$_ for |^11, 20, 30 ... 110;
printf "!%d has %d digits.\n", $_, (!$_).chars for 1000, 2000 ... 10000;
```

#### Output:
```
!0  = 0
!1  = 1
!2  = 2
!3  = 4
!4  = 10
!5  = 34
!6  = 154
!7  = 874
!8  = 5914
!9  = 46234
!10  = 409114
!20  = 128425485935180314
!30  = 9157958657951075573395300940314
!40  = 20935051082417771847631371547939998232420940314
!50  = 620960027832821612639424806694551108812720525606160920420940314
!60  = 141074930726669571000530822087000522211656242116439949000980378746128920420940314
!70  = 173639511802987526699717162409282876065556519849603157850853034644815111221599509216528920420940314
!80  = 906089587987695346534516804650290637694024830011956365184327674619752094289696314882008531991840922336528920420940314
!90  = 16695570072624210767034167688394623360733515163575864136345910335924039962404869510225723072235842668787507993136908442336528920420940314
!100  = 942786239765826579160595268206839381354754349601050974345395410407078230249590414458830117442618180732911203520208889371641659121356556442336528920420940314
!110  = 145722981061585297004706728001906071948635199234860720988658042536179281328615541936083296163475394237524337422204397431927131629058103519228197429698252556442336528920420940314
!1000 has 2565 digits.
!2000 has 5733 digits.
!3000 has 9128 digits.
!4000 has 12670 digits.
!5000 has 16322 digits.
!6000 has 20062 digits.
!7000 has 23875 digits.
!8000 has 27749 digits.
!9000 has 31678 digits.
!10000 has 35656 digits.
```


While the code above seems like a pretty decent "mathematical" way to write this,
it's far from efficient, since it's recalculating every factorial many times for each individual left factorial, not to mention for each subsequent left factorial, so it's something like an O(N^3) algorithm, not even counting the sizes of the numbers as one of the dimensions.
In Perl 6, a more idiomatic way is to write these functions as constant "triangular reduction" sequences; this works in O(N)-ish time because the sequences never have to recalculate a prior result:

```raku
constant fact = 1, |[\*] 1..*;
constant leftfact = 0, |[\+] fact;
 
printf "!%d  = %s\n", $_, leftfact[$_] for 0 ... 10, 20 ... 110;
printf "!%d has %d digits.\n", $_, leftfact[$_].chars for 1000, 2000 ... 10000;
```


Note that we just use subscripting on the list rather than an explicit function call to retrieve the desired values. If you time these two solutions, the second will run about 280 times faster than the first.