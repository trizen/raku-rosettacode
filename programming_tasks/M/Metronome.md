[1]: http://rosettacode.org/wiki/Metronome

# [Metronome][1]

This code only uses textual output, but any noise-generating commands may be substituted; as long as they are executed synchronously, and do not run longer than the specified duration, the timing loop will compensate, since the sequence operator is determining a list of absolute times for each `sleep` to target.

```perl
sub MAIN ($beats-per-minute = 72, $beats-per-bar = 4) {
    my $duration = 60 / $beats-per-minute;
    my $base-time = now + $duration;
    my $i;
 
    for $base-time, $base-time + $duration ... * -> $next-time {
        if $i++ %% $beats-per-bar {
            print "\nTICK";
        }
        else {
            print  " tick";
        }
        sleep $next-time - now;
    }
}
```


Sample run:


#### Output:
```
$ metronome 120 6

TICK tick tick tick tick tick
TICK tick tick tick tick tick
TICK tick tick tick tick tick
TICK tick tick tick tick tick
TICK tick tick^C
```