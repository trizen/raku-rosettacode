[1]: https://rosettacode.org/wiki/Superpermutation_minimisation

# [Superpermutation minimisation][1]



```perl
for 1..8 -> $len {
  my $pre = my $post = my $t = '';
  for  ('a'..'z')[^$len].permutations -> @p {
     $t = @p.join('');
     $post ~= $t        unless index($post, $t);
     $pre   = $t ~ $pre unless index($pre,  $t);
  }
  printf "%1d: %8d %8d\n", $len, $pre.chars, $post.chars;
}
```

#### Output:
```
1:        1        1
2:        4        4
3:       12       15
4:       48       64
5:      240      325
6:     1440     1956
7:    10080    13699
8:    80640   109600
```
