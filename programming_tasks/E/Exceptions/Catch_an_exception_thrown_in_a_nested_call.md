[1]: https://rosettacode.org/wiki/Exceptions/Catch_an_exception_thrown_in_a_nested_call

# [Exceptions/Catch an exception thrown in a nested call][1]



```perl
sub foo() {
    for 0..1 -> $i {
        bar $i;
        CATCH {
            when /U0/ { say "Function foo caught exception U0" }
        }
    }
}

sub bar($i) { baz $i }

sub baz($i) { die "U$i" }

foo;
```

#### Output:
```
Function foo caught exception U0
U1
  in sub baz at catch:12
  in sub bar at catch:10
  in sub foo at catch:4
  in block  at catch:14
```
