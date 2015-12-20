[1]: http://rosettacode.org/wiki/Terminal_control/Dimensions

# [Terminal control/Dimensions][1]

Using _stty_ just for the heck of it.

```perl
my $stty = qx[stty -a];
my $lines = $stty.match(/ 'rows '    <( \d+/);
my $cols  = $stty.match(/ 'columns ' <( \d+/);
say "$lines $cols";
```