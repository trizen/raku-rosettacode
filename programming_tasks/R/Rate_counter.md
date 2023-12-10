[1]: https://rosettacode.org/wiki/Rate_counter

# [Rate counter][1]



```perl
sub runrate($N where $N > 0, &todo) {
    my $n = $N;

    my $start = now;
    todo() while --$n;
    my $end = now;

    say "Start time: ", DateTime.new($start).Str;
    say "End time: ", DateTime.new($end).Str;
    my $elapsed = $end - $start;

    say "Elapsed time: $elapsed seconds";
    say "Rate: { ($N / $elapsed).fmt('%.2f') } per second\n";
}

sub factorial($n) { (state @)[$n] //= $n < 2 ?? 1 !! $n * factorial($n-1) }

runrate 100_000, { state $n = 1; factorial($n++) }

runrate 100_000, { state $n = 1; factorial($n++) }
```

#### Output:
```
Start time: 2023-04-19T08:23:50.276418Z
End time: 2023-04-19T08:23:54.716864Z
Elapsed time: 4.440445313 seconds
Rate: 22520.26 per second

Start time: 2023-04-19T08:23:54.726913Z
End time: 2023-04-19T08:23:54.798238Z
Elapsed time: 0.071324057 seconds
Rate: 1402051.48 per second
```


The `Instant` type in Perl&#160;6 is defined to be based on TAI seconds, and represented with rational numbers that are more than sufficiently accurate to represent your clock's accuracy.  The actual accuracy will depend on your clock's accuracy (even if you don't have an atomic clock in your kitchen, your smartphone can track various orbiting atomic clocks, right?) modulo the vagaries of returning the atomic time (or unreasonable facsimile) via system calls and library APIs.
