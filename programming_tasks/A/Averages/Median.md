[1]: http://rosettacode.org/wiki/Averages/Median

# [Averages/Median][1]

```perl6
sub median {
  my @a = sort @_;
  return (@a[@a.end / 2] + @a[@a / 2]) / 2;
}
```


In a slightly more compact way:

```perl6
sub median { 2 R/ [+] @_.sort[@_.end / 2, @_ / 2] }
```