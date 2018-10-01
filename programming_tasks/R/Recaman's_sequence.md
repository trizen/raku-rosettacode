[1]: https://rosettacode.org/wiki/Recaman's_sequence

# [Recaman's sequence][1]

```perl
my @recamans = 0, {
   state %seen;
   state $term;
   $term++;
   my $this = $^previous - $term;
   $this = $previous + $term unless ($this > 0) && !%seen{$this};
   %seen{$this} = True;
   $this
} … *;
 
put "First fifteen terms of Recaman's sequence: ", @recamans[^15];
 
say "First duplicate at term: a[{ @recamans.first({@recamans[^$_].Bag.values.max == 2})-1 }]";
 
my @seen;
my int $i = 0;
loop {
    next if (my int $this = @recamans[$i++]) > 1000 or @seen[$this];
    @seen[$this] = 1;
    say "Range 0..1000 covered by terms up to a[{$i - 1}]" and last if ++$ == 1001;
}
```

#### Output:
```
First fifteen terms of Recaman's sequence: 0 1 3 6 2 7 13 20 12 21 11 22 10 23 9
First duplicate at term: a[24]
Range 0..1000 covered by terms up to a[328002]
```