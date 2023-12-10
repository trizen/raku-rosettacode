[1]: https://rosettacode.org/wiki/Ulam_numbers

# [Ulam numbers][1]

```perl
my @ulams = 1, 2, &next-ulam … *;

sub next-ulam {
    state $i = 1;
    state @sums = 0,1,1;
    my $last = @ulams[$i];
    (^$i).map: { @sums[@ulams[$_] + $last]++ };
    ++$i;
    quietly ($last ^.. *).first: { @sums[$_] == 1 };
}

for 1 .. 4 {
    say "The {10**$_}th Ulam number is: ", @ulams[10**$_ - 1]
}
```

#### Output:
```
The 10th Ulam number is: 18
The 100th Ulam number is: 690
The 1000th Ulam number is: 12294
The 10000th Ulam number is: 132788
```
