[1]: http://rosettacode.org/wiki/Prime_decomposition

# [Prime decomposition][1]

```perl
constant @primes = (1..Inf).grep: *.is-prime;
 
sub factors(Int $n is copy where $n >= 2)
{
    gather for @primes -> $p {
        last if $n == 1;
 
        if $p * $p > $n {
            take $n;
            last;
        }
 
        while $n %% $p {
            take $p;
            $n div= $p;
        }
    }
}
 
multi sub MAIN('Mersenne', $n where $n >= 2)
{
    for 2..$n -> $i {
        my $m = 2**$i-1;
        say "$m: { factors($m) }";
    }
}
 
multi sub MAIN(*@numbers)
{
    for @numbers -> $n {
        say "$n: { factors($n) }";
    }
}
 
# ./factors 536870911
# ./factors Mersenne 25
 
```

#### Output:
```
536870911: 233 1103 2089

3: 3
7: 7
15: 3 5
31: 31
63: 3 3 7
127: 127
255: 3 5 17
511: 7 73
1023: 3 11 31
2047: 23 89
4095: 3 3 5 7 13
8191: 8191
16383: 3 43 127
32767: 7 31 151
65535: 3 5 17 257
131071: 131071
262143: 3 3 3 7 19 73
524287: 524287
1048575: 3 5 5 11 31 41
2097151: 7 7 127 337
4194303: 3 23 89 683
8388607: 47 178481
16777215: 3 3 5 7 13 17 241
33554431: 31 601 1801
```