[1]: http://rosettacode.org/wiki/Program_termination

# [Program termination][1]

```perl
if $problem { exit $error-code }
```


An `exit` runs all appropriate scope-leaving blocks such as `LEAVE`, `KEEP`, or `UNDO`,
followed by all `END` blocks, followed by all destructors that do more than just reclaim memory, and so cannot be skipped because they may have side effects visible outside the process. If run from an embedded interpreter, all
memory must also be reclaimed. (Perl 6 does not yet have a thread-termination policy, but will need to before we're done.)