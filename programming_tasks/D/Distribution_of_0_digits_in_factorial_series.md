[1]: https://rosettacode.org/wiki/Distribution_of_0_digits_in_factorial_series

# [Distribution of 0 digits in factorial series][1]

Works, but depressingly slow for 10000.

```perl
sub postfix:<!> (Int $n) { ( constant factorial = 1, 1, |[\*] 2..* )[$n] }
sink 10000!; # prime the iterator to allow multithreading
 
sub zs ($n) { ( constant zero-share = (^Inf).race(:32batch).map: { (.!.comb.Bag){'0'} / .!.chars } )[$n+1] }
 
.say for (
     100
    ,1000
    ,10000
).map:  -> \n { "{n}: {([+] (^n).map: *.&zs) / n}" }
```

#### Output:
```
100: 0.24675318616743216
1000: 0.20354455110316458
10000: 0.17300384824186605
```
