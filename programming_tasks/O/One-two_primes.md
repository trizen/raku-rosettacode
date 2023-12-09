[1]: https://rosettacode.org/wiki/One-two_primes

# [One-two primes][1]

### Task specific

```perl
sub condense ($n) { my $i = $n.index(2); $i ?? "(1 x $i) {$n.substr($i)}" !! $n }

sub onetwo ($d, $s='') { take $s and return unless $d; onetwo($d-1,$s~$_) for 1,2 }

sub get-onetwo ($d) { (gather onetwo $d).hyper.grep(&is-prime)[0] }

printf "%4d: %s\n", $_, get-onetwo($_) for 1..20;
printf "%4d: %s\n", $_, condense get-onetwo($_) for (1..20) »×» 100;
```

#### Output:
```
   1: 2
   2: 11
   3: 211
   4: 2111
   5: 12211
   6: 111121
   7: 1111211
   8: 11221211
   9: 111112121
  10: 1111111121
  11: 11111121121
  12: 111111211111
  13: 1111111121221
  14: 11111111112221
  15: 111111112111121
  16: 1111111112122111
  17: 11111111111112121
  18: 111111111111112111
  19: 1111111111111111111
  20: 11111111111111212121
 100: (1 x 92) 21112211
 200: (1 x 192) 21112211
 300: (1 x 288) 211121112221
 400: (1 x 390) 2111122121
 500: (1 x 488) 221222111111
 600: (1 x 590) 2112222221
 700: (1 x 689) 21111111111
 800: (1 x 787) 2122222221111
 900: (1 x 891) 222221221
1000: (1 x 988) 222122111121
1100: (1 x 1087) 2112111121111
1200: (1 x 1191) 211222211
1300: (1 x 1289) 22121221121
1400: (1 x 1388) 222211222121
1500: (1 x 1489) 21112121121
1600: (1 x 1587) 2121222122111
1700: (1 x 1688) 212121211121
1800: (1 x 1791) 221211121
1900: (1 x 1889) 22212212211
2000: (1 x 1989) 22121121211
```


### Generalized



This version will do the task requirements, but will also find (without modification):



Really, the only one that is a little tricky is the first one (0,1). That one required some specialized logic. All of the rest would work with the task specific version with different hard coded digits.



Limited the stretch to keep the run time reasonable. Finishes all in around 12 seconds on my system.

```perl
for 929,(0,1),229,(1,2),930,(1,3),931,(1,4),932,(1,5),933,(1,6),934,(1,7),935,(1,8),
    936,(1,9),937,(2,3),938,(2,7),939,(2,9),940,(3,4),941,(3,5),942,(3,7),943,(3,8),
    944,(4,7),945,(4,9),946,(5,7),947,(5,9),948,(6,7),949,(7,8),950,(7,9),951,(8,9)
  -> $oeis, $pair {

    say "\nOEIS:A036{$oeis} - Smallest n digit prime using only {$pair[0]} and {$pair[1]} (or '0' if none exists):";

    sub condense ($n) { $n.subst(/(.) {} :my $repeat=$0; ($repeat**{9..*})/, -> $/ {"($0 x {1+$1.chars}) "}) }

    sub build ($digit, $sofar='') { take $sofar and return unless $digit; build($digit-1,$sofar~$_) for |$pair }

    sub get-prime ($digits) {
        ($pair[0] ?? (gather build $digits).first: &is-prime
        !! (gather build $digits-1, $pair[1]).first: &is-prime
        ) // 0
    }

    printf "%4d: %s\n", $_, condense .&get-prime for flat 1..20, 100, 200;
}
```

#### Output:
```
OEIS:A036929 - Smallest n digit prime using only 0 and 1 (or '0' if none exists):
   1: 0
   2: 11
   3: 101
   4: 0
   5: 10111
   6: 101111
   7: 1011001
   8: 10010101
   9: 100100111
  10: 1000001011
  11: 10000001101
  12: 100000001111
  13: 1000000111001
  14: 10000000001011
  15: 100000000100101
  16: 1(0 x 10) 11101
  17: 1(0 x 12) 1101
  18: 1(0 x 11) 100111
  19: 1(0 x 13) 10011
  20: 1(0 x 12) 1100101
 100: 1(0 x 93) 101101
 200: 1(0 x 189) 1110101011

OEIS:A036229 - Smallest n digit prime using only 1 and 2 (or '0' if none exists):
   1: 2
   2: 11
   3: 211
   4: 2111
   5: 12211
   6: 111121
   7: 1111211
   8: 11221211
   9: 111112121
  10: 1111111121
  11: 11111121121
  12: 111111211111
  13: 1111111121221
  14: (1 x 10) 2221
  15: 111111112111121
  16: 1111111112122111
  17: (1 x 13) 2121
  18: (1 x 14) 2111
  19: (1 x 19) 
  20: (1 x 14) 212121
 100: (1 x 92) 21112211
 200: (1 x 192) 21112211


... many lines manually omitted ...


OEIS:A036950 - Smallest n digit prime using only 7 and 9 (or '0' if none exists):
   1: 7
   2: 79
   3: 797
   4: 0
   5: 77797
   6: 777977
   7: 7777997
   8: 77779799
   9: 777777799
  10: 7777779799
  11: 77777779979
  12: 777777779777
  13: 7777777779977
  14: (7 x 11) 977
  15: (7 x 11) 9797
  16: (7 x 11) 97799
  17: (7 x 15) 97
  18: (7 x 13) 97977
  19: (7 x 16) 997
  20: (7 x 16) 9997
 100: (7 x 93) 9979979
 200: (7 x 192) 99777779

OEIS:A036951 - Smallest n digit prime using only 8 and 9 (or '0' if none exists):
   1: 0
   2: 89
   3: 0
   4: 8999
   5: 89899
   6: 888989
   7: 8888989
   8: 88888999
   9: 888898889
  10: 8888888989
  11: 88888888999
  12: 888888898999
  13: 8888888999899
  14: (8 x 13) 9
  15: (8 x 10) 98999
  16: (8 x 10) 989999
  17: (8 x 16) 9
  18: (8 x 13) 98889
  19: (8 x 16) 989
  20: (8 x 13) 9888989
 100: (8 x 91) 998998889
 200: (8 x 190) 9888898989
```
