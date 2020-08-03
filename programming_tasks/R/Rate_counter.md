[1]: https://rosettacode.org/wiki/Rate_counter

# [Rate counter][1]

```raku
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
 
sub factorial($n) { (state @)[$n] //= $n < 2 ?? 1 !! $n * factorial($n-1) }
 
runrate 10000, { state $n = 1; factorial($n++) }
 
runrate 10000, { state $n = 1; factorial($n++) }
```

#### Output:
```
Start time: 2013-03-08T20:57:02Z
End time: 2013-03-08T20:57:03Z
Elapsed time: 1.5467497 seconds
Rate: 6465.17 per second

Start time: 2013-03-08T20:57:03Z
End time: 2013-03-08T20:57:04Z
Elapsed time: 0.7036318 seconds
Rate: 14211.98 per second
```


The `Instant` type in Perl 6 is defined to be based on TAI seconds, and represented with rational numbers that are more than sufficiently accurate to represent your clock's accuracy. The actual accuracy will depend on your clock's accuracy (even if you don't have an atomic clock in your kitchen, your smartphone can track various orbiting atomic clocks, right?) modulo the vagaries of returning the atomic time (or unreasonable facsimile) via system calls and library APIs.