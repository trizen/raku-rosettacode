[1]: https://rosettacode.org/wiki/Inventory_sequence

# [Inventory sequence][1]

```perl
use Lingua::EN::Numbers;

my ($i, %i) = 0;

my @inventory = (^∞).map: {
    my $count = %i{$i} // 0;
    $i = $count ?? $i+1 !! 0;
    ++%i{$count};
    $count
}

say "Inventory sequence, first 100 elements:\n" ~
  @inventory[^100].batch(20)».fmt("%2d").join: "\n";

say '';

for (1..10).map: * × 1000 {
    my $k = @inventory.first: * >= $_, :k;
    printf "First element >= %6s is %6s in position: %s\n",
      .&comma, @inventory[$k].&comma, comma $k;
}


use SVG;
use SVG::Plot;

my @x = ^10000;

'Inventory-raku.svg'.IO.spurt:
 SVG.serialize: SVG::Plot.new(
    background  => 'white',
    width       => 1000,
    height      => 600,
    plot-width  => 950,
    plot-height => 550,
    x           => @x,
    values      => [@inventory[@x],],
    title       => "Inventory Sequence - First {+@x} values (zero indexed)",
).plot: :lines;
```

#### Output:
```
Inventory sequence, first 100 elements:
 0  1  1  0  2  2  2  0  3  2  4  1  1  0  4  4  4  1  4  0
 5  5  4  1  6  2  1  0  6  7  5  1  6  3  3  1  0  7  9  5
 3  6  4  4  2  0  8  9  6  4  9  4  5  2  1  3  0  9 10  7
 5 10  6  6  3  1  4  2  0 10 11  8  6 11  6  9  3  2  5  3
 2  0 11 11 10  8 11  7  9  4  3  6  4  5  0 12 11 10  9 13

First element >=  1,000 is  1,001 in position: 24,255
First element >=  2,000 is  2,009 in position: 43,301
First element >=  3,000 is  3,001 in position: 61,708
First element >=  4,000 is  4,003 in position: 81,456
First element >=  5,000 is  5,021 in position: 98,704
First element >=  6,000 is  6,009 in position: 121,342
First element >=  7,000 is  7,035 in position: 151,756
First element >=  8,000 is  8,036 in position: 168,804
First element >=  9,000 is  9,014 in position: 184,428
First element >= 10,000 is 10,007 in position: 201,788
```


*converted to a .png to reduce size for display here:*



<br clear="all" />
