[1]: https://rosettacode.org/wiki/Prime_numbers_p_which_sum_of_prime_numbers_less_or_equal_to_p_is_prime

# [Prime numbers p which sum of prime numbers less or equal to p is prime][1]

Trivial variation of [Summarize primes](https://rosettacode.org/wiki/Summarize_primes#Raku) task. Modified to do double duty.

```perl
use Lingua::EN::Numbers;
 
my @primes    = grep *.is-prime, ^Inf;
my @primesums = [\+] @primes;
say "{.elems} cumulative prime sums:\n",
    .map( -> $p {
        sprintf "The sum of the first %3d (up to {@primes[$p]}) is prime: %s",
        1 + $p, comma @primesums[$p]
      }
    ).join("\n")
    given grep { @primesums[$_].is-prime }, ^1000;
```

#### Output:
```
76 cumulative prime sums:
The sum of the first   1 (up to 2) is prime: 2
The sum of the first   2 (up to 3) is prime: 5
The sum of the first   4 (up to 7) is prime: 17
The sum of the first   6 (up to 13) is prime: 41
The sum of the first  12 (up to 37) is prime: 197
The sum of the first  14 (up to 43) is prime: 281
The sum of the first  60 (up to 281) is prime: 7,699
The sum of the first  64 (up to 311) is prime: 8,893
The sum of the first  96 (up to 503) is prime: 22,039
The sum of the first 100 (up to 541) is prime: 24,133
The sum of the first 102 (up to 557) is prime: 25,237
The sum of the first 108 (up to 593) is prime: 28,697
The sum of the first 114 (up to 619) is prime: 32,353
The sum of the first 122 (up to 673) is prime: 37,561
The sum of the first 124 (up to 683) is prime: 38,921
The sum of the first 130 (up to 733) is prime: 43,201
The sum of the first 132 (up to 743) is prime: 44,683
The sum of the first 146 (up to 839) is prime: 55,837
The sum of the first 152 (up to 881) is prime: 61,027
The sum of the first 158 (up to 929) is prime: 66,463
The sum of the first 162 (up to 953) is prime: 70,241
The sum of the first 178 (up to 1061) is prime: 86,453
The sum of the first 192 (up to 1163) is prime: 102,001
The sum of the first 198 (up to 1213) is prime: 109,147
The sum of the first 204 (up to 1249) is prime: 116,533
The sum of the first 206 (up to 1277) is prime: 119,069
The sum of the first 208 (up to 1283) is prime: 121,631
The sum of the first 214 (up to 1307) is prime: 129,419
The sum of the first 216 (up to 1321) is prime: 132,059
The sum of the first 296 (up to 1949) is prime: 263,171
The sum of the first 308 (up to 2029) is prime: 287,137
The sum of the first 326 (up to 2161) is prime: 325,019
The sum of the first 328 (up to 2203) is prime: 329,401
The sum of the first 330 (up to 2213) is prime: 333,821
The sum of the first 332 (up to 2237) is prime: 338,279
The sum of the first 334 (up to 2243) is prime: 342,761
The sum of the first 342 (up to 2297) is prime: 360,979
The sum of the first 350 (up to 2357) is prime: 379,667
The sum of the first 356 (up to 2393) is prime: 393,961
The sum of the first 358 (up to 2411) is prime: 398,771
The sum of the first 426 (up to 2957) is prime: 581,921
The sum of the first 446 (up to 3137) is prime: 642,869
The sum of the first 458 (up to 3251) is prime: 681,257
The sum of the first 460 (up to 3257) is prime: 687,767
The sum of the first 464 (up to 3301) is prime: 700,897
The sum of the first 480 (up to 3413) is prime: 754,573
The sum of the first 484 (up to 3461) is prime: 768,373
The sum of the first 488 (up to 3491) is prime: 782,263
The sum of the first 512 (up to 3671) is prime: 868,151
The sum of the first 530 (up to 3821) is prime: 935,507
The sum of the first 536 (up to 3863) is prime: 958,577
The sum of the first 548 (up to 3947) is prime: 1,005,551
The sum of the first 568 (up to 4129) is prime: 1,086,557
The sum of the first 620 (up to 4583) is prime: 1,313,041
The sum of the first 630 (up to 4657) is prime: 1,359,329
The sum of the first 676 (up to 5051) is prime: 1,583,293
The sum of the first 680 (up to 5087) is prime: 1,603,597
The sum of the first 696 (up to 5233) is prime: 1,686,239
The sum of the first 708 (up to 5351) is prime: 1,749,833
The sum of the first 734 (up to 5563) is prime: 1,891,889
The sum of the first 762 (up to 5807) is prime: 2,051,167
The sum of the first 768 (up to 5849) is prime: 2,086,159
The sum of the first 776 (up to 5897) is prime: 2,133,121
The sum of the first 780 (up to 5939) is prime: 2,156,813
The sum of the first 784 (up to 6007) is prime: 2,180,741
The sum of the first 808 (up to 6211) is prime: 2,327,399
The sum of the first 814 (up to 6263) is prime: 2,364,833
The sum of the first 820 (up to 6301) is prime: 2,402,537
The sum of the first 836 (up to 6427) is prime: 2,504,323
The sum of the first 844 (up to 6529) is prime: 2,556,187
The sum of the first 848 (up to 6563) is prime: 2,582,401
The sum of the first 852 (up to 6581) is prime: 2,608,699
The sum of the first 926 (up to 7243) is prime: 3,120,833
The sum of the first 942 (up to 7433) is prime: 3,238,237
The sum of the first 984 (up to 7757) is prime: 3,557,303
The sum of the first 992 (up to 7853) is prime: 3,619,807
```
