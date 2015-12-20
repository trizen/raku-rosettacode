[1]: http://rosettacode.org/wiki/Synchronous_concurrency

# [Synchronous concurrency][1]

```perl6
sub MAIN ($infile) {
    $infile.IO.lines ==> printer() ==> my $count;
    say "printed $count lines";
}
 
sub printer(*@lines) {
    my $lines;
    for @lines {
	.say;
	++$lines;
    }
    $lines;
}
```


Concurrent units are established by use of the feed operator, which works much like an in-process object pipe. Since the final feed goes to a variable declaration that belongs to the outer thread, it serves as a backchannel from the printer thread. In this case the outer thread signals the desire for a line count by terminating the pipe to the printing thread.
(Note: soon these will be implemented with real threads in Perl 6, but this is currently emulated with coroutine semantics, aka lazy lists.)