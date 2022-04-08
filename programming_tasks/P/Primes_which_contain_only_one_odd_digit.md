[1]: https://rosettacode.org/wiki/Primes_which_contain_only_one_odd_digit

# [Primes which contain only one odd digit][1]

```perl
put display ^1000 .grep: { ($_ % 2) && .is-prime && (.comb[^(*-1)].all %% 2) }
 
sub display ($list, :$cols = 10, :$fmt = '%6d', :$title = "{+$list} matching:\n" )   {
    cache $list;
    $title ~ $list.batch($cols)».fmt($fmt).join: "\n"
}
```

#### Output:
```
45 matching:
     3      5      7     23     29     41     43     47     61     67
    83     89    223    227    229    241    263    269    281    283
   401    409    421    443    449    461    463    467    487    601
   607    641    643    647    661    683    809    821    823    827
   829    863    881    883    887
```
