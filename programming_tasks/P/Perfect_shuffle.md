[1]: http://rosettacode.org/wiki/Perfect_shuffle

# [Perfect shuffle][1]

```perl
sub perfect-shuffle (@deck) {
    my $mid = @deck / 2;
    flat @deck[0 ..^ $mid] Z @deck[$mid .. *];
}
 
for 8, 24, 52, 100, 1020, 1024, 10000 -> $size {
    my @deck = ^$size;
    my $n;
    repeat until [<] @deck {
        $n++;
        @deck = perfect-shuffle @deck;
    }
 
    printf "%5d cards: %4d\n", $size, $n;
}
```

#### Output:
```
    8 cards:    3
   24 cards:   11
   52 cards:    8
  100 cards:   30
 1020 cards: 1018
 1024 cards:   10
10000 cards:  300
```