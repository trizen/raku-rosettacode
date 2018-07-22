[1]: https://rosettacode.org/wiki/Fibonacci_sequence

# [Fibonacci sequence][1]

### List Generator



This constructs the fibonacci sequence as a lazy infinite list.

```perl
constant @fib = 0, 1, *+* ... *;
```


If you really need a function for it:

```perl
sub fib ($n) { @fib[$n] }
```


To support negative indices:

```perl
constant @neg-fib = 0, 1, *-* ... *;
sub fib ($n) { $n >= 0 ?? @fib[$n] !! @neg-fib[-$n] }
```


### Iterative

```perl
sub fib (Int $n --> Int) {
    $n > 1 or return $n;
    my ($prev, $this) = 0, 1;
    ($prev, $this) = $this, $this + $prev for 1 ..^ $n;
    return $this;
}
```


### Recursive

```perl
proto fib (Int $n --> Int) {*}
multi fib (0)  { 0 }
multi fib (1)  { 1 }
multi fib ($n) { fib($n - 1) + fib($n - 2) }
```


### Analytic

```perl
sub fib (Int $n --> Int) {
    constant φ1 = 1 / constant φ = (1 + sqrt 5)/2;
    constant invsqrt5 = 1 / sqrt 5;
 
    floor invsqrt5 * (φ**$n + φ1**$n);
}
```