[1]: http://rosettacode.org/wiki/Abbreviations,_automatic

# [Abbreviations, automatic][1]

Saving the "Days of Week, Also Known As" table to a local file DoWAKA.txt. Note: lines that have duplicate day names will get ∞ as the minimum number of characters, as there is no amount of characters that can be entered to distinguish the days uniquely. It is somewhat unclear as to what is meant by "return a null string". I have chosen to return Nil.



Note that this is using a previous version of the date file that has erroneous duplicate day names (see line 90). Since the effort was already expended to catch such problems, it may as well be demonstrated.

```perl
sub auto-abbreviate ( Str $string ) {
    return Nil unless my @days = $string.words;
    my $max = @days».chars.max;
    for 0 .. $max {
        return $_  if +@days».substr(0, $_).Set == 7;
        return '∞' if $_ == $max;
    }
}
 
# Testing
 say ++$, ') ', .&auto-abbreviate, '  ', $_ for './DoWAKA.txt'.IO.lines;
```