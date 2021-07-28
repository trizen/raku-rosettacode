[1]: https://rosettacode.org/wiki/Prime_numbers_which_contain_123

# [Prime numbers which contain 123][1]

```perl
my @p123 = ^∞ .grep: { .is-prime && .contains: 123 };
 
put display @p123[^(@p123.first: * > 1e5, :k)];
 
put "\nCount up to 1e6: ", ~ +@p123[^(@p123.first: * > 1e6, :k)];
 
sub display ($list, :$cols = 10, :$fmt = '%6d', :$title = "{+$list} matching:\n" )   {
    cache $list;
    $title ~ $list.batch($cols)».fmt($fmt).join: "\n"
}
```

#### Output:
```
46 matching:
  1123   1231   1237   8123  11239  12301  12323  12329  12343  12347
 12373  12377  12379  12391  17123  20123  22123  28123  29123  31123
 31231  31237  34123  37123  40123  41231  41233  44123  47123  49123
 50123  51239  56123  59123  61231  64123  65123  70123  71233  71237
 76123  81233  81239  89123  91237  98123

Count up to 1e6: 451
```
