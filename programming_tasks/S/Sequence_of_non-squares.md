[1]: http://rosettacode.org/wiki/Sequence_of_non-squares

# [Sequence of non-squares][1]

```perl6
sub nth_term (Int $n) { $n + round sqrt $n }
 
say nth_term $_ for 1 .. 22;
 
loop (my $i = 1; $i <= 1_000_000; $i++) {
    $i.&nth_term.sqrt %% 1 and say "nth_term($i) is square.";
}
```


With a recent Rakudo using MoarVM the last test takes under a minute, but older Rakudos may take much longer.