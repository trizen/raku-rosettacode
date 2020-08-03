[1]: https://rosettacode.org/wiki/Metallic_ratios

# [Metallic ratios][1]

Note: Arrays, Lists and Sequences are zero indexed in Perl 6.

```perl
use Rat::Precise;
use Lingua::EN::Numbers;
 
sub lucas ($b) { 1, 1, * + $b * * … * }
 
sub metallic ($seq, $places = 32) {
    my $n = 0;
    my $last = 0;
    loop {
        my $approx = FatRat.new($seq[$n + 1], $seq[$n]);
        my $this = $approx.precise($places, :z);
        last if $this eq $last;
        $last = $this;
        $n++;
    }
    $last, $n
}
 
sub display ($value, $n) {
    "Approximated value:", $value, "Reached after {$n} iterations: " ~
    "{ordinal-digit $n}/{ordinal-digit $n - 1} element."
}
 
for <Platinum Golden Silver Bronze Copper Nickel Aluminum Iron Tin Lead>.kv
  -> \b, $name {
    my $lucas = lucas b;
    print "\nLucas sequence for $name ratio; where b = {b}:\nFirst 15 elements: ";
    say join ', ', $lucas[^15];
    say join ' ', display |metallic($lucas);
}
 
# Stretch goal
say join "\n", "\nGolden ratio to 256 decimal places:", display |metallic lucas(1), 256;
```

#### Output:
```
Lucas sequence for Platinum ratio; where b = 0:
First 15 elements: 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1
Approximated value: 1.00000000000000000000000000000000 Reached after 1 iterations: 1st/0th element.

Lucas sequence for Golden ratio; where b = 1:
First 15 elements: 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144, 233, 377, 610
Approximated value: 1.61803398874989484820458683436564 Reached after 78 iterations: 78th/77th element.

Lucas sequence for Silver ratio; where b = 2:
First 15 elements: 1, 1, 3, 7, 17, 41, 99, 239, 577, 1393, 3363, 8119, 19601, 47321, 114243
Approximated value: 2.41421356237309504880168872420970 Reached after 44 iterations: 44th/43rd element.

Lucas sequence for Bronze ratio; where b = 3:
First 15 elements: 1, 1, 4, 13, 43, 142, 469, 1549, 5116, 16897, 55807, 184318, 608761, 2010601, 6640564
Approximated value: 3.30277563773199464655961063373525 Reached after 34 iterations: 34th/33rd element.

Lucas sequence for Copper ratio; where b = 4:
First 15 elements: 1, 1, 5, 21, 89, 377, 1597, 6765, 28657, 121393, 514229, 2178309, 9227465, 39088169, 165580141
Approximated value: 4.23606797749978969640917366873128 Reached after 28 iterations: 28th/27th element.

Lucas sequence for Nickel ratio; where b = 5:
First 15 elements: 1, 1, 6, 31, 161, 836, 4341, 22541, 117046, 607771, 3155901, 16387276, 85092281, 441848681, 2294335686
Approximated value: 5.19258240356725201562535524577016 Reached after 25 iterations: 25th/24th element.

Lucas sequence for Aluminum ratio; where b = 6:
First 15 elements: 1, 1, 7, 43, 265, 1633, 10063, 62011, 382129, 2354785, 14510839, 89419819, 551029753, 3395598337, 20924619775
Approximated value: 6.16227766016837933199889354443272 Reached after 23 iterations: 23rd/22nd element.

Lucas sequence for Iron ratio; where b = 7:
First 15 elements: 1, 1, 8, 57, 407, 2906, 20749, 148149, 1057792, 7552693, 53926643, 385039194, 2749201001, 19629446201, 140155324408
Approximated value: 7.14005494464025913554865124576352 Reached after 22 iterations: 22nd/21st element.

Lucas sequence for Tin ratio; where b = 8:
First 15 elements: 1, 1, 9, 73, 593, 4817, 39129, 317849, 2581921, 20973217, 170367657, 1383914473, 11241683441, 91317382001, 741780739449
Approximated value: 8.12310562561766054982140985597408 Reached after 20 iterations: 20th/19th element.

Lucas sequence for Lead ratio; where b = 9:
First 15 elements: 1, 1, 10, 91, 829, 7552, 68797, 626725, 5709322, 52010623, 473804929, 4316254984, 39320099785, 358197153049, 3263094477226
Approximated value: 9.10977222864644365500113714088140 Reached after 20 iterations: 20th/19th element.

Golden ratio to 256 decimal places:
Approximated value:
1.6180339887498948482045868343656381177203091798057628621354486227052604628189024497072072041893911374847540880753868917521266338622235369317931800607667263544333890865959395829056383226613199282902678806752087668925017116962070322210432162695486262963136144
Reached after 615 iterations: 615th/614th element.
```
