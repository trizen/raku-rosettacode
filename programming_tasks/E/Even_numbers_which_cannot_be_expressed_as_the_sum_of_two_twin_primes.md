[1]: https://rosettacode.org/wiki/Even_numbers_which_cannot_be_expressed_as_the_sum_of_two_twin_primes

# [Even numbers which cannot be expressed as the sum of two twin primes][1]

```perl
my $threshold = 10000;

my @twins = unique flat (3..$threshold).grep(&is-prime).map: { $_, $_+2 if ($_+2).is-prime };
my @sums;

map {++@sums[$_]}, (@twins X+ @twins);
display 'Non twin prime sums:', @sums;

map {++@sums[$_]}, flat 2, (1 X+ @twins);
display "\nNon twin prime sums (including 1):", @sums;

sub display ($msg, @p) {
    put "$msg\n" ~ @p[^$threshold].kv.map(-> $k, $v { $k if ($k %% 2) && !$v })\
    .skip(1).batch(10)».fmt("%4d").join: "\n"
}
```

#### Output:
```
Non twin prime sums:
   2    4   94   96   98  400  402  404  514  516
 518  784  786  788  904  906  908 1114 1116 1118
1144 1146 1148 1264 1266 1268 1354 1356 1358 3244
3246 3248 4204 4206 4208

Non twin prime sums (including 1):
  94   96   98  400  402  404  514  516  518  784
 786  788  904  906  908 1114 1116 1118 1144 1146
1148 1264 1266 1268 1354 1356 1358 3244 3246 3248
4204 4206 4208
```
