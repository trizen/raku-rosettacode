[1]: https://rosettacode.org/wiki/Ramanujan_primes

# [Ramanujan primes][1]

All timings are purely informational. Will vary by system specs and load.



### Pure Raku

```perl
use Math::Primesieve;
use Lingua::EN::Numbers;
 
my $primes = Math::Primesieve.new;
 
my @mem;
 
sub ramanujan-prime (\n) {
   1 + (1..(4×n × (4×n).log / 2.log).floor).first: :end, -> \x {
       my \y = x div 2;
       ((@mem[x] //= $primes.count(x)) - (@mem[y] //= $primes.count(y))) < n
   }
}
 
say 'First 100:';
say (1..100).map( &ramanujan-prime ).batch(10)».&comma».fmt("%6s").join: "\n";
say "\n 1,000th: { comma 1000.&ramanujan-prime }";
say "10,000th: {  comma 10000.&ramanujan-prime }";
say (now - INIT now).fmt('%.3f') ~ ' seconds';
```

#### Output:
```
First 100:
     2     11     17     29     41     47     59     67     71     97
   101    107    127    149    151    167    179    181    227    229
   233    239    241    263    269    281    307    311    347    349
   367    373    401    409    419    431    433    439    461    487
   491    503    569    571    587    593    599    601    607    641
   643    647    653    659    677    719    727    739    751    769
   809    821    823    827    853    857    881    937    941    947
   967    983  1,009  1,019  1,021  1,031  1,049  1,051  1,061  1,063
 1,087  1,091  1,097  1,103  1,151  1,163  1,187  1,217  1,229  1,249
 1,277  1,289  1,297  1,301  1,367  1,373  1,423  1,427  1,429  1,439

 1,000th: 19,403
10,000th: 242,057
18.405 seconds
```


### ntheory library

```perl
use ntheory:from<Perl5> <ramanujan_primes nth_ramanujan_prime>;
use Lingua::EN::Numbers;
 
say 'First 100:';
say ramanujan_primes( nth_ramanujan_prime(100) ).batch(10)».&comma».fmt("%6s").join: "\n";
 
for (2..12).map: {exp $_, 10} -> $limit {
    say "\n{tc ordinal $limit}: { comma nth_ramanujan_prime($limit) }";
}
 
say (now - INIT now).fmt('%.3f') ~ ' seconds';
```

#### Output:
```
First 100:
     2     11     17     29     41     47     59     67     71     97
   101    107    127    149    151    167    179    181    227    229
   233    239    241    263    269    281    307    311    347    349
   367    373    401    409    419    431    433    439    461    487
   491    503    569    571    587    593    599    601    607    641
   643    647    653    659    677    719    727    739    751    769
   809    821    823    827    853    857    881    937    941    947
   967    983  1,009  1,019  1,021  1,031  1,049  1,051  1,061  1,063
 1,087  1,091  1,097  1,103  1,151  1,163  1,187  1,217  1,229  1,249
 1,277  1,289  1,297  1,301  1,367  1,373  1,423  1,427  1,429  1,439

One hundredth: 1,439

One thousandth: 19,403

Ten thousandth: 242,057

One hundred thousandth: 2,916,539

One millionth: 34,072,993

Ten millionth: 389,433,437

One hundred millionth: 4,378,259,731

One billionth: 48,597,112,639

Ten billionth: 533,902,884,973

One hundred billionth: 5,816,713,968,619

One trillionth: 62,929,891,461,461
15.572 seconds
```
