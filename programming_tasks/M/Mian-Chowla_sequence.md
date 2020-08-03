[1]: https://rosettacode.org/wiki/Mian-Chowla_sequence

# [Mian-Chowla sequence][1]

```raku
my @mian-chowla = 1, |(2..Inf).map: -> $test {
    state $index = 1;
    state %sums  = 2 => 1;
    my $next;
    my %these;
    ((|@mian-chowla[^$index], $test) »+» $test).map: { ++$next and last if %sums{$_}:exists; ++%these{$_} };
    next if $next;
    %sums.push: %these;
    ++$index;
    $test
};
 
put "First 30 terms in the Mian–Chowla sequence:\n", @mian-chowla[^30];
put "\nTerms 91 through 100:\n", @mian-chowla[90..99];
```

#### Output:
```
First 30 terms in the Mian–Chowla sequence:
1 2 4 8 13 21 31 45 66 81 97 123 148 182 204 252 290 361 401 475 565 593 662 775 822 916 970 1016 1159 1312

Terms 91 through 100:
22526 23291 23564 23881 24596 24768 25631 26037 26255 27219
```