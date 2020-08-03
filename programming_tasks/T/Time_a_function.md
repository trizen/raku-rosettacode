[1]: https://rosettacode.org/wiki/Time_a_function

# [Time a function][1]

Follows modern trend toward measuring wall-clock time, since CPU time is becoming rather ill-defined in the age of multicore, and doesn't reflect IO overhead in any case.

```raku
my $start = now;
(^100000).pick(1000);
say now - $start;
```

#### Output:
```
0.02301709
```