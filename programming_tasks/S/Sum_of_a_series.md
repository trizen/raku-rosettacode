[1]: http://rosettacode.org/wiki/Sum_of_a_series

# [Sum of a series][1]

(Some of these work with [rakudo](http://rosettacode.org/wiki/Rakudo), and others with [niecza](http://rosettacode.org/wiki/Niecza). Eventually they'll all work everywhere...)



In general, the `$n`th partial sum of a series whose terms are given by a unary function `&amp;f` is

```perl
[+] map &f, 1 .. $n
```


So what's needed in this case is

```perl
say [+] map { 1 / $^n**2 }, 1 .. 1000;
```


Or, using the "hyper" metaoperator to vectorize, we can use a more "point free" style while keeping traditional precedence:

```perl
say [+] 1 «/« (1..1000) »**» 2;
```


Or we can use the <tt>X</tt> "cross" metaoperator, which is convenient even if one side or the other is a scalar. In this case, we demonstrate a scalar on either side:

```perl
say [+] 1 X/ (1..1000 X** 2);
```


Note that cross ops are parsed as list infix precedence rather than using the precedence of the base op as hypers do. Hence the difference in parenthesization.



In a lazy language like Perl 6, it's generally considered a stronger abstraction to write the correct infinite sequence, and then take the part of it you're interested in.
Here we define an infinite sequence of partial sums (by adding a backslash into the reduction to make it look "triangular"), then take the 1000th term of that:

```perl
constant @x = [\+] 0, { 1 / ++(state $n) ** 2 } ... *;
say @x[1000];  # prints 1.64393456668156
```


Note that infinite constant sequences can be lazily generated in Perl 6, or this wouldn't work so well...



A cleaner style is to combine these approaches with a more FP look:

```perl
constant ζish = [\+] map -> \𝑖 { 1 / 𝑖**2 }, 1..*;
say ζish[1000];
```


Perhaps the cleanest way is to just define the zeta function and evaluate it for s=2, possibly using memoization:

```perl
sub ζ($s) is cached { [\+] 1..* X** -$s }
say ζ(2)[1000];
```


Notice how the thus-defined zeta function returns a lazy list of approximated values, which is arguably the closest we can get from the mathematical definition.



Finally, if list comprehensions are your hammer, you can nail it this way:

```perl
say [+] (1 / $_**2 for 1..1000);
```


That's fine for a single result, but if you're going to be evaluating the sequence multiple times, you don't want to be recalculating the sum each time, so it's more efficient to define the sequence as a constant to let the run-time automatically cache those values already calculated.