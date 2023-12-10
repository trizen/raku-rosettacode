[1]: https://rosettacode.org/wiki/History_variables

# [History variables][1]



```perl
class HistoryVar {
    has @.history;
    has $!var handles <Str gist FETCH Numeric>;
    method STORE($val) is rw {
        push @.history, [now, $!var];
        $!var = $val;
    }
}

my \foo = HistoryVar.new;

foo = 1;
foo = 2;
foo += 3;
foo = 42;

.say for foo.history;
say "Current value: {foo}";
```

#### Output:
```
[Instant:1523396079.685629 (Any)]
[Instant:1523396079.686844 1]
[Instant:1523396079.687130 2]
[Instant:1523396079.687302 5]
Current value: 42
```
