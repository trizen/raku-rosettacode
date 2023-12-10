[1]: https://rosettacode.org/wiki/9_billion_names_of_God_the_integer

# [9 billion names of God the integer][1]


To save a bunch of memory, this algorithm throws away all the numbers that it knows it's not going to use again, on the assumption that the function will only be called with increasing values of $n.  (It could easily be made to recalculate if it notices a regression.)

```perl
my @todo = $[1];
my @sums = 0;
sub nextrow($n) {
    for +@todo .. $n -> $l {
        my $r = [];
        for reverse ^$l -> $x {
            my @x := @todo[$x];
            if @x {
                $r.push: @sums[$x] += @x.shift;
            }
            else {
                $r.push: @sums[$x];
            }
        }
        @todo.push($r);
    }
    @todo[$n];
}

say "rows:";
say .fmt('%2d'), ": ", nextrow($_)[] for 1..25;


my @names-of-God = 1, { partition-sum ++$ } … *;
my @names-of-God-adder = lazy [\+] flat 1, ( (1 .. *) Z (1 .. *).map: * × 2 + 1 );
sub partition-sum ($n) {
    sum @names-of-God[$n X- @names-of-God-adder[^(@names-of-God-adder.first: * > $n, :k)]]
        Z× (flat (1, 1, -1, -1) xx *)
}

say "\nsums:";
for 23, 123, 1234, 12345 {
    put $_, "\t",  @names-of-God[$_];
}
```

#### Output:
```
rows:
 1: [1]
 2: [1 1]
 3: [1 1 1]
 4: [1 2 1 1]
 5: [1 2 2 1 1]
 6: [1 3 3 2 1 1]
 7: [1 3 4 3 2 1 1]
 8: [1 4 5 5 3 2 1 1]
 9: [1 4 7 6 5 3 2 1 1]
10: [1 5 8 9 7 5 3 2 1 1]
11: [1 5 10 11 10 7 5 3 2 1 1]
12: [1 6 12 15 13 11 7 5 3 2 1 1]
13: [1 6 14 18 18 14 11 7 5 3 2 1 1]
14: [1 7 16 23 23 20 15 11 7 5 3 2 1 1]
15: [1 7 19 27 30 26 21 15 11 7 5 3 2 1 1]
16: [1 8 21 34 37 35 28 22 15 11 7 5 3 2 1 1]
17: [1 8 24 39 47 44 38 29 22 15 11 7 5 3 2 1 1]
18: [1 9 27 47 57 58 49 40 30 22 15 11 7 5 3 2 1 1]
19: [1 9 30 54 70 71 65 52 41 30 22 15 11 7 5 3 2 1 1]
20: [1 10 33 64 84 90 82 70 54 42 30 22 15 11 7 5 3 2 1 1]
21: [1 10 37 72 101 110 105 89 73 55 42 30 22 15 11 7 5 3 2 1 1]
22: [1 11 40 84 119 136 131 116 94 75 56 42 30 22 15 11 7 5 3 2 1 1]
23: [1 11 44 94 141 163 164 146 123 97 76 56 42 30 22 15 11 7 5 3 2 1 1]
24: [1 12 48 108 164 199 201 186 157 128 99 77 56 42 30 22 15 11 7 5 3 2 1 1]
25: [1 12 52 120 192 235 248 230 201 164 131 100 77 56 42 30 22 15 11 7 5 3 2 1 1]

sums:
23      1255
123     2552338241
1234    156978797223733228787865722354959930
12345   69420357953926116819562977205209384460667673094671463620270321700806074195845953959951425306140971942519870679768681736
```
