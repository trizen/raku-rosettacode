[1]: https://rosettacode.org/wiki/Metered_concurrency

# [Metered concurrency][1]

Uses a buffered channel to hand out a limited number of tickets.

```perl
class Semaphore {
    has $.tickets = Channel.new;
    method new ($max) {
        my $s = self.bless; 
        $s.tickets.send(True) xx $max;
        $s;
    }
    method acquire { $.tickets.receive }
    method release { $.tickets.send(True) }
}
 
sub MAIN ($units = 5, $max = 2) {
    my $sem = Semaphore.new($max);
 
    my @units = do for ^$units -> $u {
        start {
            $sem.acquire; say "unit $u acquired";
            sleep 2;
            $sem.release; say "unit $u released";
        }
    }
    await @units;
}
```

#### Output:
```
unit 0 acquired
unit 1 acquired
unit 0 released
unit 1 released
unit 3 acquired
unit 2 acquired
unit 3 released
unit 2 released
unit 4 acquired
unit 4 released
```