[1]: https://rosettacode.org/wiki/Perfect_totient_numbers

# [Perfect totient numbers][1]

```perl
my \𝜑  = Nil, |(1..*).hyper.map: -> $t { +(^$t).grep: * gcd $t == 1 };
my \𝜑𝜑 = Nil, |(2..*).grep: -> $p { $p == sum 𝜑[$p], { 𝜑[$_] } … 1 };
 
put "The first twenty Perfect totient numbers:\n",  𝜑𝜑[1..20];
```

#### Output:
```
The first twenty Perfect totient numbers:
3 9 15 27 39 81 111 183 243 255 327 363 471 729 2187 2199 3063 4359 4375 5571
```