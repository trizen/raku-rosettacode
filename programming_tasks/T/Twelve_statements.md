[1]: https://rosettacode.org/wiki/Twelve_statements

# [Twelve statements][1]



```perl
sub infix:<→> ($protasis, $apodosis) { !$protasis or $apodosis }
 
my @tests =
    { .end == 12 and all(.[1..12]) === any(True, False) },
    { 3 == [+] .[7..12] },
    { 2 == [+] .[2,4...12] },
    { .[5] → all .[6,7] },
    { none .[2,3,4] },
    { 4 == [+] .[1,3...11] },
    { one .[2,3] },
    { .[7] → all .[5,6] },
    { 3 == [+] .[1..6] },
    { all .[11,12] },
    { one .[7,8,9] },
    { 4 == [+] .[1..11] },
;

my @solutions;
my @misses;

for [X] (True, False) xx 12 {
    my @assert = Nil, |$_;
    my @result = Nil, |@tests.map({ ?.(@assert) });
    my @true = @assert.grep(?*, :k);
    my @cons = (@assert Z=== @result).grep(!*, :k);
    given @cons {
        when 0 { push @solutions, "<{@true}> is consistent."; }
        when 1 { push @misses, "<{@true}> implies { "¬" if !@result[~$_] }$_." }
    }
}

.say for @solutions;
say "";
say "Near misses:";
.say for @misses;
```

#### Output:
```
<1 3 4 6 7 11> is consistent.

Near misses:
<1 2 4 7 8 9> implies ¬8.
<1 2 4 7 9 10> implies ¬10.
<1 2 4 7 9 12> implies ¬12.
<1 3 4 6 7 9> implies ¬9.
<1 3 4 8 9> implies 7.
<1 4 6 8 9> implies ¬6.
<1 4 8 10 11 12> implies ¬12.
<1 4> implies 8.
<1 5 6 9 11> implies 8.
<1 5 8 10 11 12> implies ¬12.
<1 5 8 11> implies 12.
<1 5 8> implies 11.
<1 5> implies 8.
<4 8 10 11 12> implies 1.
<5 8 10 11 12> implies 1.
<5 8 11> implies 1.
```
