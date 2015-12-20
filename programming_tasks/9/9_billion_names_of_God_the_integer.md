[1]: http://rosettacode.org/wiki/9_billion_names_of_God_the_integer

# [9 billion names of God the integer][1]

To save a bunch of memory, this algorithm throws away all the numbers that it knows it's not going to use again, on the assumption that the function will only be called with increasing values of $n. (It could easily be made to recalculate if it notices a regression.)



Note: We only run to 10000 currently because mono gets major GC indigestion at 10200 or so...

```perl
my @todo = [1];
my @sums = 0;
sub nextrow($n) {
    for +@todo .. $n -> $l {
        @sums[$l] = 0;
        print $l,"\r" if $l < $n;
        my $r = [];
        for reverse ^$l -> $x {
            my @x := @todo[$x];
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
say .fmt('%2d'), ": ", nextrow($_)[] for 1..10;
 
 
say "\nsums:";
for 23, 123, 1234, 10000 {
    say $_, "\t", [+] nextrow($_)[];
}
```

#### Output:
```
rows:
 1: 1
 2: 1 1
 3: 1 1 1
 4: 1 2 1 1
 5: 1 2 2 1 1
 6: 1 3 3 2 1 1
 7: 1 3 4 3 2 1 1
 8: 1 4 5 5 3 2 1 1
 9: 1 4 7 6 5 3 2 1 1
10: 1 5 8 9 7 5 3 2 1 1

sums:
23      1255
123     2552338241
1234    156978797223733228787865722354959930
10000   36167251325636293988820471890953695495016030339315650422081868605887952568754066420592310556052906916435144
```