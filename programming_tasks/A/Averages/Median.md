[1]: https://rosettacode.org/wiki/Averages/Median

# [Averages/Median][1]

```raku
sub median {
  my @a = sort @_;
  return (@a[(*-1) div 2] + @a[* div 2]) / 2;
}
```


Notes:





In a slightly more compact way:

```raku
sub median { @_.sort[(*-1)/2, */2].sum / 2 }
```