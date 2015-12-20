[1]: http://rosettacode.org/wiki/Fibonacci_sequence

# [Fibonacci sequence][1]

This constructs the fibonacci sequence as a lazy infinite list.

```perl
my constant @fib = 0, 1, *+* ... *;
```


If you really need a function for it:

```perl
sub fib ($n) { @fib[$n] }
```


To support negative indices:

```perl
my constant @neg_fib = 0, 1, *-* ... *;
sub fib ($n) { $n >= 0 and @fib[$n] or @neg_fib[-$n]; }
```
```perl
sub fib (Int $n --> Int) {
    $n > 1 or return $n;
    my ($prev, $this) = 0, 1;
    ($prev, $this) = $this, $this + $prev for 1 ..^ $n;
    return $this;
}
```
```perl
proto fib (Int $n --> Int) is cached {*}
multi fib (0)  { 0 }
multi fib (1)  { 1 }
multi fib ($n) { fib($n - 1) + fib($n - 2) }
```
```perl
sub fib (Int $n --> Int) {
    constant φ1 = 1 / constant φ = (1 + sqrt 5)/2;
    constant invsqrt5 = 1 / sqrt 5;
 
    floor invsqrt5 * (φ**$n + φ1**$n);
}
```