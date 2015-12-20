[1]: http://rosettacode.org/wiki/History_variables

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
 
my $foo := HistoryVar.new;
 
$foo = 1;
$foo = 2;
$foo += 3;
$foo = 42;
 
.say for $foo.history;
say "Current value: $foo";
```

#### Output:
```
Instant:1387608436.366940 (Any)
Instant:1387608436.388883 1
Instant:1387608436.402433 2
Instant:1387608436.413677 5
Current value: 42
```