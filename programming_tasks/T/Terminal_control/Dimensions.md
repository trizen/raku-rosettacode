[1]: https://rosettacode.org/wiki/Terminal_control/Dimensions

# [Terminal control/Dimensions][1]


Using *stty* just for the heck of it.

```perl
my $stty = qx[stty -a];
my $lines = $stty.match(/ 'rows '    <( \d+/);
my $cols  = $stty.match(/ 'columns ' <( \d+/);
say "$lines $cols";
```
