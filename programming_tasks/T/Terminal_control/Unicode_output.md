[1]: https://rosettacode.org/wiki/Terminal_control/Unicode_output

# [Terminal control/Unicode output][1]

```perl
die "Terminal can't handle UTF-8"
    unless first(*.defined, %*ENV<LC_ALL LC_CTYPE LANG>) ~~ /:i 'utf-8'/;
say "△";
```

#### Output:
```
△
```