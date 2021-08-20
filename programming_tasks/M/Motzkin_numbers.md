[1]: https://rosettacode.org/wiki/Motzkin_numbers

# [Motzkin numbers][1]

### Using binomial coefficients and Catalan numbers

```perl
use Lingua::EN::Numbers;
 
constant \C = 1, |[\×] (2, 6 … ∞) Z/ 2 .. *;
 
sub binomial { [×] ($^n … 0) Z/ 1 .. $^p }
 
my \𝐌 = 1, |(1..∞).map: -> \𝐧 { sum ^𝐧 .map( -> \𝐤 { C[𝐤] × binomial 𝐧, 2×𝐤 } ) };
 
say " 𝐧          𝐌[𝐧]            Prime?";
𝐌[^42].kv.map: { printf "%2d %24s  %s\n", $^k, $^v.&comma, $v.is-prime };
```

#### Output:
```
 𝐧          𝐌[𝐧]            Prime?
 0                        1  False
 1                        1  False
 2                        2  True
 3                        4  False
 4                        9  False
 5                       21  False
 6                       51  False
 7                      127  True
 8                      323  False
 9                      835  False
10                    2,188  False
11                    5,798  False
12                   15,511  True
13                   41,835  False
14                  113,634  False
15                  310,572  False
16                  853,467  False
17                2,356,779  False
18                6,536,382  False
19               18,199,284  False
20               50,852,019  False
21              142,547,559  False
22              400,763,223  False
23            1,129,760,415  False
24            3,192,727,797  False
25            9,043,402,501  False
26           25,669,818,476  False
27           73,007,772,802  False
28          208,023,278,209  False
29          593,742,784,829  False
30        1,697,385,471,211  False
31        4,859,761,676,391  False
32       13,933,569,346,707  False
33       40,002,464,776,083  False
34      114,988,706,524,270  False
35      330,931,069,469,828  False
36      953,467,954,114,363  True
37    2,750,016,719,520,991  False
38    7,939,655,757,745,265  False
39   22,944,749,046,030,949  False
40   66,368,199,913,921,497  False
41  192,137,918,101,841,817  False
```


### Using recurrence relationship

```perl
use Lingua::EN::Numbers;
 
my \𝐌 = 1, 1, { state $i = 2; ++$i; ($^b × (2 × $i - 1) + $^a × (3 × $i - 6)) ÷ ($i + 1) } … *;
 
say " 𝐧          𝐌[𝐧]            Prime?";
𝐌[^42].kv.map: { printf "%2d %24s  %s\n", $^k, $^v.&comma, $v.is-prime };
```


Same output
