[1]: http://rosettacode.org/wiki/Zeckendorf_number_representation

# [Zeckendorf number representation][1]

```perl6
printf "%2d: %8s\n", $_, zeckendorf($_) for 0 .. 20;
 
multi zeckendorf(0) { '0' }
multi zeckendorf($n is copy) {
    constant FIBS = 1,2, *+* ... *;
    [~] map {
        $n -= $_ if my $digit = $n >= $_;
        +$digit;
    }, reverse FIBS ...^ * > $n;
}
```

#### Output:
```
 0:        0
 1:        1
 2:       10
 3:      100
 4:      101
 5:     1000
 6:     1001
 7:     1010
 8:    10000
 9:    10001
10:    10010
11:    10100
12:    10101
13:   100000
14:   100001
15:   100010
16:   100100
17:   100101
18:   101000
19:   101001
20:   101010
```