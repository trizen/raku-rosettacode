[1]: https://rosettacode.org/wiki/Fortunate_numbers

# [Fortunate numbers][1]

Limit of 75 primorials to get first 50 unique fortunates is arbitrary, found through trial and error.

```perl
my @primorials = [\*] grep *.is-prime, ^∞;
 
say display :title("First 50 distinct fortunate numbers:\n"),
   (squish sort @primorials[^75].hyper.map: -> $primorial {
       (2..∞).first: (* + $primorial).is-prime
   })[^50];
 
sub display ($list, :$cols = 10, :$fmt = '%6d', :$title = "{+$list} matching:\n") {
    cache $list;
    $title ~ $list.batch($cols)».fmt($fmt).join: "\n"
}
```

#### Output:
```
First 50 distinct fortunate numbers:
     3      5      7     13     17     19     23     37     47     59
    61     67     71     79     89    101    103    107    109    127
   151    157    163    167    191    197    199    223    229    233
   239    271    277    283    293    307    311    313    331    353
   373    379    383    397    401    409    419    421    439    443
```
