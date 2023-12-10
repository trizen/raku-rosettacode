[1]: https://rosettacode.org/wiki/Weird_numbers

# [Weird numbers][1]



```perl
sub abundant (\x) {
    my @l = x.is-prime ?? 1 !! flat
    1, (2 .. x.sqrt.floor).map: -> \d {
         my \y = x div d;
         next if y * d !== x;
         d !== y ?? (d, y) !! d
    };
    (my $s = @l.sum) > x ?? ($s, |@l.sort(-*)) !! ();
}

my @weird = (2, 4, {|($_ + 4, $_ + 6)} ... *).map: -> $n {
    my ($sum, @div) = $n.&abundant;
    next unless $sum;        # Weird number must be abundant, skip it if it isn't.
    next if $sum / $n > 1.1; # There aren't any weird numbers with a sum:number ratio greater than 1.08 or so.
    if $n >= 10430 and ($n %% 70) and ($n div 70).is-prime {
        # It's weird. All numbers of the form 70 * (a prime 149 or larger) are weird
    } else {
        my $next;
        my $l = @div.shift;
        ++$next and last if $_.sum == $n - $l for @div.combinations;
        next if $next;
    }
    $n
}

put "The first 25 weird numbers:\n", @weird[^25];
```

#### Output:
```
The first 25 weird numbers:
70 836 4030 5830 7192 7912 9272 10430 10570 10792 10990 11410 11690 12110 12530 12670 13370 13510 13790 13930 14770 15610 15890 16030 16310
```
