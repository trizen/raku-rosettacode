[1]: http://rosettacode.org/wiki/Sleep

# [Sleep][1]

The `sleep` function argument is in units of seconds, but these may be fractional (to the limits of your system's clock).

```perl
my $sec = prompt("Sleep for how many microfortnights? ") * 1.2096;
say "Sleeping...";
sleep $sec;
say "Awake!";
```


Note that 1.2096 is a rational number in Perl&#160;6, not floating point, so precision can be maintained even when dealing with very small powers of ten.