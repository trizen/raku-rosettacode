[1]: http://rosettacode.org/wiki/Convert_seconds_to_compound_duration

# [Convert seconds to compound duration][1]

The built-in `polymod` method (which is a generalization of the `divmod` function known from other languages), is a perfect match for a task like this:

```perl6
sub compound-duration ($seconds) {
    ($seconds.polymod(60, 60, 24, 7) Z <sec min hr d wk>)\
    .grep(*[0]).reverse.join(", ")
}
```


Demonstration:

```perl6
for 7259, 86400, 6000000 {
    say "{.fmt: '%7d'} sec  =  {compound-duration $_}";
}
```

#### Output:
```
   7259 sec  =  2 hr, 59 sec
  86400 sec  =  1 d
6000000 sec  =  9 wk, 6 d, 10 hr, 40 min
```