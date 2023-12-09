[1]: https://rosettacode.org/wiki/Radical_of_an_integer

# [Radical of an integer][1]

```perl
use Prime::Factor;
use List::Divvy;
use Lingua::EN::Numbers;

sub radical ($_) { [×] unique .&prime-factors }

say "First fifty radicals:\n" ~
  (1..50).map({.&radical}).batch(10)».fmt("%2d").join: "\n";
say '';

printf "Radical for %7s => %7s\n", .&comma, comma .&radical
  for 99999, 499999, 999999;

my %rad = 1 => 1;
my $limit = 1e6.Int;

%rad.push: $_ for (2..$limit).race(:1000batch).map: {(unique .&prime-factors).elems => $_};

say "\nRadical factor count breakdown, 1 through {comma $limit}:";
say .key ~ " => {comma +.value}" for sort %rad;

my @primes = (2..$limit).grep: &is-prime;

my int $powers;
@primes.&upto($limit.sqrt.floor).map: -> $p {
   for (2..*) { ($p ** $_) < $limit ?? ++$powers !! last }
}

say qq:to/RADICAL/;

    Up to {comma $limit}:
    Primes: {comma +@primes}
    Powers:    $powers
    Plus 1:      1
    Total:  {comma 1 + $powers + @primes}
    RADICAL
```

#### Output:
```
First fifty radicals:
 1  2  3  2  5  6  7  2  3 10
11  6 13 14 15  2 17  6 19 10
21 22 23  6  5 26  3 14 29 30
31  2 33 34 35  6 37 38 39 10
41 42 43 22 15 46 47  6  7 10

Radical for  99,999 =>  33,333
Radical for 499,999 =>   3,937
Radical for 999,999 => 111,111

Radical factor count breakdown, 1 through 1,000,000:
1 => 78,735
2 => 288,726
3 => 379,720
4 => 208,034
5 => 42,492
6 => 2,285
7 => 8

Up to 1,000,000:
Primes: 78,498
Powers:    236
Plus 1:      1
Total:  78,735
```
