[1]: https://rosettacode.org/wiki/Humble_numbers

# [Humble numbers][1]



```perl
sub smooth-numbers (*@list) {
    cache my \Smooth := gather {
        my %i = (flat @list) Z=> (Smooth.iterator for ^@list);
        my %n = (flat @list) Z=> 1 xx *;

        loop {
            take my $n := %n{*}.min;

            for @list -> \k {
                %n{k} = %i{k}.pull-one * k if %n{k} == $n;
            }
        }
    }
}

my $humble := smooth-numbers(2,3,5,7);

put $humble[^50];
say '';

my $upto = 50;
my $digits = 1;
my $count;

$humble.map: -> \h {
    ++$count and next if h.chars == $digits;
    printf "Digits: %2d - Count: %s\n", $digits++, $count;
    $count = 1;
    last if $digits > $upto;
}
```

#### Output:
```
1 2 3 4 5 6 7 8 9 10 12 14 15 16 18 20 21 24 25 27 28 30 32 35 36 40 42 45 48 49 50 54 56 60 63 64 70 72 75 80 81 84 90 96 98 100 105 108 112 120

Digits:  1 - Count: 9
Digits:  2 - Count: 36
Digits:  3 - Count: 95
Digits:  4 - Count: 197
Digits:  5 - Count: 356
Digits:  6 - Count: 579
Digits:  7 - Count: 882
Digits:  8 - Count: 1272
Digits:  9 - Count: 1767
Digits: 10 - Count: 2381
Digits: 11 - Count: 3113
Digits: 12 - Count: 3984
Digits: 13 - Count: 5002
Digits: 14 - Count: 6187
Digits: 15 - Count: 7545
Digits: 16 - Count: 9081
Digits: 17 - Count: 10815
Digits: 18 - Count: 12759
Digits: 19 - Count: 14927
Digits: 20 - Count: 17323
Digits: 21 - Count: 19960
Digits: 22 - Count: 22853
Digits: 23 - Count: 26015
Digits: 24 - Count: 29458
Digits: 25 - Count: 33188
Digits: 26 - Count: 37222
Digits: 27 - Count: 41568
Digits: 28 - Count: 46245
Digits: 29 - Count: 51254
Digits: 30 - Count: 56618
Digits: 31 - Count: 62338
Digits: 32 - Count: 68437
Digits: 33 - Count: 74917
Digits: 34 - Count: 81793
Digits: 35 - Count: 89083
Digits: 36 - Count: 96786
Digits: 37 - Count: 104926
Digits: 38 - Count: 113511
Digits: 39 - Count: 122546
Digits: 40 - Count: 132054
Digits: 41 - Count: 142038
Digits: 42 - Count: 152515
Digits: 43 - Count: 163497
Digits: 44 - Count: 174986
Digits: 45 - Count: 187004
Digits: 46 - Count: 199565
Digits: 47 - Count: 212675
Digits: 48 - Count: 226346
Digits: 49 - Count: 240590
Digits: 50 - Count: 255415
```
