[1]: https://rosettacode.org/wiki/Stack_traces

# [Stack traces][1]

```raku
sub g { say Backtrace.new.concise }
sub f { g }
sub MAIN { f }
```

#### Output:
```
  in sub g at bt:1
  in sub f at bt:2
  in sub MAIN at bt:3
```