[1]: https://rosettacode.org/wiki/Bell_numbers

# [Bell numbers][1]





### via Aitken's array

```perl
 my @Aitkens-array = lazy [1], -> @b {
     my @c = @b.tail;
     @c.push: @b[$_] + @c[$_] for ^@b;
     @c
 } ... *;

 my @Bell-numbers = @Aitkens-array.map: { .head };

say "First fifteen and fiftieth Bell numbers:";
printf "%2d: %s\n", 1+$_, @Bell-numbers[$_] for flat ^15, 49;

say "\nFirst ten rows of Aitken's array:";
.say for @Aitkens-array[^10];
```

#### Output:
```
First fifteen and fiftieth Bell numbers:
 1: 1
 2: 1
 3: 2
 4: 5
 5: 15
 6: 52
 7: 203
 8: 877
 9: 4140
10: 21147
11: 115975
12: 678570
13: 4213597
14: 27644437
15: 190899322
50: 10726137154573358400342215518590002633917247281

First ten rows of Aitken's array:
[1]
[1 2]
[2 3 5]
[5 7 10 15]
[15 20 27 37 52]
[52 67 87 114 151 203]
[203 255 322 409 523 674 877]
[877 1080 1335 1657 2066 2589 3263 4140]
[4140 5017 6097 7432 9089 11155 13744 17007 21147]
[21147 25287 30304 36401 43833 52922 64077 77821 94828 115975]
```


### via Recurrence relation

```perl
sub binomial { [*] ($^n … 0) Z/ 1 .. $^p }

my @bell = 1, -> *@s { [+] @s »*« @s.keys.map: { binomial(@s-1, $_) }  } … *;

.say for @bell[^15], @bell[50 - 1];
```

#### Output:
```
(1 1 2 5 15 52 203 877 4140 21147 115975 678570 4213597 27644437 190899322)
10726137154573358400342215518590002633917247281
```


### via Stirling sums

```perl
my @Stirling_numbers_of_the_second_kind =
    (1,),
    { (0, |@^last) »+« (|(@^last »*« @^last.keys), 0) } … *
;
my @bell = @Stirling_numbers_of_the_second_kind.map: *.sum;

.say for @bell.head(15), @bell[50 - 1];
```

#### Output:
```
(1 1 2 5 15 52 203 877 4140 21147 115975 678570 4213597 27644437 190899322)
10726137154573358400342215518590002633917247281 
```
