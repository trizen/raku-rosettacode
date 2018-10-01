[1]: https://rosettacode.org/wiki/Loops/Wrong_ranges

# [Loops/Wrong ranges][1]

It would be odd to call ANY of these sequences "wrong" in Perl 6. Perl 6 specifically has built in capability of working with infinite sequences. Just because a sequence is infinite, doesn't mean you can't define it, work with it or use values from it. Sure, if you try to reify the whole thing you may be waiting a while, but there is nothing preventing you from using a portion of it.



Perl 6 sequence definitions *specifically* allow "ending points" that may never occur in the sequence. Since that is the case, you don't even really **need** to specify a stop value. You can just say stop at "whatever". Whatever is spelled "**\***" in Perl 6.



There is additional syntax you can add to stop at the nearest value, last value previous or first value successor to the "stop value" (Note I didn't say less than or greater than the stop value since the sequence can be ascending, descending or non-monotonic).



Also note: The iterator function for the sequence is literally a function. It is any expression that produces a value. These sequences all use simple arithmatic increments but that is not a limitation of the sequence operator.

```perl
# Given sequence definitions
#   start  stop  inc.   Comment
for   -2,    2,    1, # Normal
      -2,    2,    0, # Zero increment
      -2,    2,   -1, # Increments away from stop value
      -2,    2,   10, # First increment is beyond stop value
       2,   -2,    1, # Start more than stop: positive increment
       2,    2,    1, # Start equal stop: positive increment
       2,    2,    0, # Start equal stop: zero increment
       0,    0,    0, # Start equal stop equal zero: zero increment
 
# Additional "problematic" sequences
       1,  Inf,    3, # Endpoint literally at infinity
       0,    π,  τ/8, # Floating point numbers
     1.4,    *, -7.1  # Whatever
 
  -> $start, $stop, $inc {
    my $seq = flat ($start, *+$inc … $stop);
    printf "Start: %3s, Stop: %3s, Increment: %3s | ", $start, $stop.Str, $inc;
    # only show up to the first 15 elements of possibly infinite sequences
    put $seq[^15].grep: +*.defined
}
 
# For that matter the start and end values don't need to be numeric either. Both
# or either can be a function, list, or other object. Really anything that a
# "successor" function can be defined for and produces a value.
 
# Start with a list, iterate by multiplying the previous 3 terms together
# and end with a term defined by a function.
put 1, -.5, 2.sqrt, * * * * * … *.abs < 1e-2;
 
# Start with an array, iterate by rotating, end when 0 is in the last place.
say [0,1,2,3,4,5], *.rotate(-1) … !*.tail;
 
# Iterate strings backwards.
put 'xp' … 'xf';
```

#### Output:
```
Start:  -2, Stop:   2, Increment:   1 | -2 -1 0 1 2
Start:  -2, Stop:   2, Increment:   0 | -2 -2 -2 -2 -2 -2 -2 -2 -2 -2 -2 -2 -2 -2 -2
Start:  -2, Stop:   2, Increment:  -1 | -2 -3 -4 -5 -6 -7 -8 -9 -10 -11 -12 -13 -14 -15 -16
Start:  -2, Stop:   2, Increment:  10 | -2 8 18 28 38 48 58 68 78 88 98 108 118 128 138
Start:   2, Stop:  -2, Increment:   1 | 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16
Start:   2, Stop:   2, Increment:   1 | 2
Start:   2, Stop:   2, Increment:   0 | 2
Start:   0, Stop:   0, Increment:   0 | 0
Start:   1, Stop: Inf, Increment:   3 | 1 4 7 10 13 16 19 22 25 28 31 34 37 40 43
Start:   0, Stop: 3.141592653589793, Increment: 0.7853981633974483 | 0 0.7853981633974483 1.5707963267948966 2.356194490192345 3.141592653589793
Start: 1.4, Stop:   *, Increment: -7.1 | 1.4 -5.7 -12.8 -19.9 -27 -34.1 -41.2 -48.3 -55.4 -62.5 -69.6 -76.7 -83.8 -90.9 -98
1 -0.5 1.4142135623730951 -0.7071067811865476 0.5000000000000001 -0.5000000000000002 0.176776695296637 -0.04419417382415928 0.0039062500000000095
([0 1 2 3 4 5] [5 0 1 2 3 4] [4 5 0 1 2 3] [3 4 5 0 1 2] [2 3 4 5 0 1] [1 2 3 4 5 0])
xp xo xn xm xl xk xj xi xh xg xf
```