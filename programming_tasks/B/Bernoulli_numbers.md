[1]: https://rosettacode.org/wiki/Bernoulli_numbers

# [Bernoulli numbers][1]

### Simple



First, a straighforward implementation of the naïve algorithm in the task description.

```raku
sub bernoulli($n) {
    my @a;
    for 0..$n -> $m {
        @a[$m] = FatRat.new(1, $m + 1);
        for reverse 1..$m -> $j {
          @a[$j - 1] = $j * (@a[$j - 1] - @a[$j]);
        }
    }
    return @a[0];
}
 
constant @bpairs = grep *.value.so, ($_ => bernoulli($_) for 0..60);
 
my $width = [max] @bpairs.map: *.value.numerator.chars;
my $form = "B(%2d) = \%{$width}d/%d\n";
 
printf $form, .key, .value.nude for @bpairs;
```

#### Output:
```
B( 0) =                                            1/1
B( 1) =                                            1/2
B( 2) =                                            1/6
B( 4) =                                           -1/30
B( 6) =                                            1/42
B( 8) =                                           -1/30
B(10) =                                            5/66
B(12) =                                         -691/2730
B(14) =                                            7/6
B(16) =                                        -3617/510
B(18) =                                        43867/798
B(20) =                                      -174611/330
B(22) =                                       854513/138
B(24) =                                   -236364091/2730
B(26) =                                      8553103/6
B(28) =                                 -23749461029/870
B(30) =                                8615841276005/14322
B(32) =                               -7709321041217/510
B(34) =                                2577687858367/6
B(36) =                        -26315271553053477373/1919190
B(38) =                             2929993913841559/6
B(40) =                       -261082718496449122051/13530
B(42) =                       1520097643918070802691/1806
B(44) =                     -27833269579301024235023/690
B(46) =                     596451111593912163277961/282
B(48) =                -5609403368997817686249127547/46410
B(50) =                  495057205241079648212477525/66
B(52) =              -801165718135489957347924991853/1590
B(54) =             29149963634884862421418123812691/798
B(56) =          -2479392929313226753685415739663229/870
B(58) =          84483613348880041862046775994036021/354
B(60) = -1215233140483755572040304994079820246041491/56786730
```


### With memoization



Here is a much faster way, following the Perl solution that avoids recalculating previous values each time through the function. We do this in Perl 6 by not defining it as a function at all, but by defining it as an infinite sequence that we can read however many values we like from (52, in this case, to get up to B(100)). In this solution we've also avoided subscripting operations; rather we use a sequence operator (`...`) iterated over the list of the previous solution to find the next solution. We reverse the array in this case to make reference to the previous value in the list more natural, which means we take the last value of the list rather than the first value, and do so conditionally to avoid 0 values.

```raku
constant bernoulli = gather {
    my @a;
    for 0..* -> $m {
        @a = FatRat.new(1, $m + 1),
                -> $prev {
                    my $j = @a.elems;
                    $j * (@a.shift - $prev);
                } ... { not @a.elems }
        take $m => @a[*-1] if @a[*-1];
    }
}
 
constant @bpairs = bernoulli[^52];
 
my $width = [max] @bpairs.map: *.value.numerator.chars;
my $form = "B(%d)\t= \%{$width}d/%d\n";
 
printf $form, .key, .value.nude for @bpairs;
```

#### Output:
```
B(0)    =                                                                                    1/1
B(1)    =                                                                                    1/2
B(2)    =                                                                                    1/6
B(4)    =                                                                                   -1/30
B(6)    =                                                                                    1/42
B(8)    =                                                                                   -1/30
B(10)   =                                                                                    5/66
B(12)   =                                                                                 -691/2730
B(14)   =                                                                                    7/6
B(16)   =                                                                                -3617/510
B(18)   =                                                                                43867/798
B(20)   =                                                                              -174611/330
B(22)   =                                                                               854513/138
B(24)   =                                                                           -236364091/2730
B(26)   =                                                                              8553103/6
B(28)   =                                                                         -23749461029/870
B(30)   =                                                                        8615841276005/14322
B(32)   =                                                                       -7709321041217/510
B(34)   =                                                                        2577687858367/6
B(36)   =                                                                -26315271553053477373/1919190
B(38)   =                                                                     2929993913841559/6
B(40)   =                                                               -261082718496449122051/13530
B(42)   =                                                               1520097643918070802691/1806
B(44)   =                                                             -27833269579301024235023/690
B(46)   =                                                             596451111593912163277961/282
B(48)   =                                                        -5609403368997817686249127547/46410
B(50)   =                                                          495057205241079648212477525/66
B(52)   =                                                      -801165718135489957347924991853/1590
B(54)   =                                                     29149963634884862421418123812691/798
B(56)   =                                                  -2479392929313226753685415739663229/870
B(58)   =                                                  84483613348880041862046775994036021/354
B(60)   =                                         -1215233140483755572040304994079820246041491/56786730
B(62)   =                                               12300585434086858541953039857403386151/6
B(64)   =                                          -106783830147866529886385444979142647942017/510
B(66)   =                                       1472600022126335654051619428551932342241899101/64722
B(68)   =                                        -78773130858718728141909149208474606244347001/30
B(70)   =                                    1505381347333367003803076567377857208511438160235/4686
B(72)   =                             -5827954961669944110438277244641067365282488301844260429/140100870
B(74)   =                                   34152417289221168014330073731472635186688307783087/6
B(76)   =                               -24655088825935372707687196040585199904365267828865801/30
B(78)   =                            414846365575400828295179035549542073492199375372400483487/3318
B(80)   =                       -4603784299479457646935574969019046849794257872751288919656867/230010
B(82)   =                        1677014149185145836823154509786269900207736027570253414881613/498
B(84)   =                 -2024576195935290360231131160111731009989917391198090877281083932477/3404310
B(86)   =                      660714619417678653573847847426261496277830686653388931761996983/6
B(88)   =              -1311426488674017507995511424019311843345750275572028644296919890574047/61410
B(90)   =            1179057279021082799884123351249215083775254949669647116231545215727922535/272118
B(92)   =           -1295585948207537527989427828538576749659341483719435143023316326829946247/1410
B(94)   =            1220813806579744469607301679413201203958508415202696621436215105284649447/6
B(96)   =   -211600449597266513097597728109824233673043954389060234150638733420050668349987259/4501770
B(98)   =        67908260672905495624051117546403605607342195728504487509073961249992947058239/6
B(100)  = -94598037819122125295227433069493721872702841533066936133385696204311395415197247711/33330
```


### Functional



And if you're a pure enough FP programmer to dislike destroying and reconstructing the array each time, here's the same algorithm without side effects. We use zip with the pair constructor `=>` to keep values associated with their indices. This provides sufficient local information that we can define our own binary operator "bop" to reduce between each two terms, using the "triangle" form (called "scan" in Haskell) to return the intermediate results that will be important to compute the next Bernoulli number.

```raku
sub infix:<bop>(\prev, \this) {
    this.key => this.key * (this.value - prev.value)
}
 
sub next-bernoulli ( (:key($pm), :value(@pa)) ) {
    $pm + 1 => [
        map *.value,
        [\bop] ($pm + 2 ... 1) Z=> FatRat.new(1, $pm + 2), |@pa
    ]
}
 
constant bernoulli =
    grep *.value,
    map { .key => .value[*-1] },
    (0 => [FatRat.new(1,1)], &next-bernoulli ... *)
;
 
constant @bpairs = bernoulli[^52];
 
my $width = [max] @bpairs.map: *.value.numerator.chars;
my $form = "B(%d)\t= \%{$width}d/%d\n";
 
printf $form, .key, .value.nude for @bpairs;
```


Same output as memoization example