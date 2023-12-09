[1]: https://rosettacode.org/wiki/CalmoSoft_primes

# [CalmoSoft primes][1]

Longest sliding window prime sums

```perl
use Lingua::EN::Numbers;

sub sliding-window(@list, $window) { (^(+@list - $window)).map: { @list[$_ .. $_+$window] } }

for flat (1e2, 1e3, 1e4, 1e5).map: { (1, 2.5, 5) »×» $_ } -> $upto {

    my @primes = (^$upto).grep: &is-prime;

    for +@primes ... 1 {
        my @sums = @primes.&sliding-window($_).grep: { .sum.is-prime }
        next unless @sums;
        say "\nFor primes up to {$upto.Int.&cardinal}:\nLongest sequence of consecutive primes yielding a prime sum: elements: {comma 1+$_}";
        for @sums {  say " {join '...', .[0..5, *-5..*]».&comma».join(' + ')}, sum: {.sum.&comma}" }
        last
    }
}
```

#### Output:
```
For primes up to one hundred:
Longest sequence of consecutive primes yielding a prime sum: elements: 21
 7 + 11 + 13 + 17 + 19 + 23...71 + 73 + 79 + 83 + 89, sum: 953

For primes up to two hundred fifty:
Longest sequence of consecutive primes yielding a prime sum: elements: 49
 11 + 13 + 17 + 19 + 23 + 29...227 + 229 + 233 + 239 + 241, sum: 5,813

For primes up to five hundred:
Longest sequence of consecutive primes yielding a prime sum: elements: 85
 31 + 37 + 41 + 43 + 47 + 53...467 + 479 + 487 + 491 + 499, sum: 21,407

For primes up to one thousand:
Longest sequence of consecutive primes yielding a prime sum: elements: 163
 13 + 17 + 19 + 23 + 29 + 31...971 + 977 + 983 + 991 + 997, sum: 76,099

For primes up to two thousand, five hundred:
Longest sequence of consecutive primes yielding a prime sum: elements: 359
 7 + 11 + 13 + 17 + 19 + 23...2,411 + 2,417 + 2,423 + 2,437 + 2,441, sum: 408,479

For primes up to five thousand:
Longest sequence of consecutive primes yielding a prime sum: elements: 665
 7 + 11 + 13 + 17 + 19 + 23...4,967 + 4,969 + 4,973 + 4,987 + 4,993, sum: 1,543,127

For primes up to ten thousand:
Longest sequence of consecutive primes yielding a prime sum: elements: 1,223
 3 + 5 + 7 + 11 + 13 + 17...9,887 + 9,901 + 9,907 + 9,923 + 9,929, sum: 5,686,633
 7 + 11 + 13 + 17 + 19 + 23...9,907 + 9,923 + 9,929 + 9,931 + 9,941, sum: 5,706,497

For primes up to twenty-five thousand:
Longest sequence of consecutive primes yielding a prime sum: elements: 2,759
 7 + 11 + 13 + 17 + 19 + 23...24,967 + 24,971 + 24,977 + 24,979 + 24,989, sum: 32,405,707

For primes up to fifty thousand:
Longest sequence of consecutive primes yielding a prime sum: elements: 5,131
 5 + 7 + 11 + 13 + 17 + 19...49,943 + 49,957 + 49,991 + 49,993 + 49,999, sum: 121,013,303

For primes up to one hundred thousand:
Longest sequence of consecutive primes yielding a prime sum: elements: 9,590
 2 + 3 + 5 + 7 + 11 + 13...99,907 + 99,923 + 99,929 + 99,961 + 99,971, sum: 454,196,557

For primes up to two hundred fifty thousand:
Longest sequence of consecutive primes yielding a prime sum: elements: 22,041
 7 + 11 + 13 + 17 + 19 + 23...249,947 + 249,967 + 249,971 + 249,973 + 249,989, sum: 2,623,031,141

For primes up to five hundred thousand:
Longest sequence of consecutive primes yielding a prime sum: elements: 41,530
 2 + 3 + 5 + 7 + 11 + 13...499,801 + 499,819 + 499,853 + 499,879 + 499,883, sum: 9,910,236,647
```
