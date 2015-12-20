[1]: http://rosettacode.org/wiki/Handle_a_signal

# [Handle a signal][1]

We note with glee that the task does not require us to print <em>consecutive</em> integers, so we'll print Fibonacci numbers instead. `:-)`

```perl
signal(Signal::SIGINT).tap: {
    note "Took { now - INIT now } seconds.";
    exit;
}
 
for 0, 1, *+* ... * {
    sleep 0.5;
    .say;
}
```

#### Output:
```
0
1
1
2
3
5
8
13
21
34
55
89
^CTook 6.3437449 seconds.
Aborted
```