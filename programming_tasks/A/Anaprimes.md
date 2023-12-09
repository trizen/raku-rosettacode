[1]: https://rosettacode.org/wiki/Anaprimes

# [Anaprimes][1]

9 digit is slooow. I didn't have the patience for 10.

```perl
use Lingua::EN::Numbers;
use Math::Primesieve;
use List::Allmax;

my $p = Math::Primesieve.new;

for 3 .. 9 {
    my @largest = $p.primes(10**($_-1), 10**$_).classify(*.comb.sort.join).List.&all-max(:by(+*.value)).values;

    put "\nLargest group of anaprimes before {cardinal 10 ** $_}: {+@largest[0].value} members.";
    put 'First: ', ' Last: ' Z~ .value[0, *-1] for sort @largest;
}
```

#### Output:
```
Largest group of anaprimes before one thousand: 4 members.
First: 149  Last: 941
First: 179  Last: 971
First: 379  Last: 937

Largest group of anaprimes before ten thousand: 11 members.
First: 1237  Last: 7321
First: 1279  Last: 9721

Largest group of anaprimes before one hundred thousand: 39 members.
First: 13789  Last: 98731

Largest group of anaprimes before one million: 148 members.
First: 123479  Last: 974213

Largest group of anaprimes before ten million: 731 members.
First: 1235789  Last: 9875321

Largest group of anaprimes before one hundred million: 4,333 members.
First: 12345769  Last: 97654321

Largest group of anaprimes before one billion: 26,519 members.
First: 102345697  Last: 976542103
```
