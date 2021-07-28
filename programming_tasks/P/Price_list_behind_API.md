[1]: https://rosettacode.org/wiki/Price_list_behind_API

# [Price list behind API][1]

```perl
# 20210208 Raku programming solution
 
my \minDelta = 1;
 
sub getMaxPrice { @_.max }
 
sub getPRangeCount(@prices,\min,\max) { +@prices.grep: min ≤ * ≤ max }
 
sub get5000(@prices, $min, $max is copy, \n) {
   my $count = getPRangeCount(@prices, $min, $max);
   my $delta = ($max - $min) / 2;
   while $count != n && $delta ≥ minDelta/2 {
      $count > n ?? ($max -= $delta) !! ($max += $delta);
      $count = getPRangeCount(@prices, $min, $max); 
      $delta /= 2;
   }
   $max, $count
}
 
sub getAll5000(@prices, \min, \max, \n) {
   my ( $pmax, $pcount ) = get5000(@prices, min, max, n);
   my @res = [ min, $pmax, $pcount ] , ;
   while $pmax < max {
      my $pmin = $pmax + 1;
      ( $pmax, $pcount ) = get5000(@prices, $pmin, max, n);
      $pcount == 0 and note "Price list from $pmin has too many duplicates.";
      @res.push: [ $pmin, $pmax, $pcount ];
   } 
   @res
} 
 
my $numPrices = (99000..101001).roll;
my \maxPrice  = 1e5;
my @prices    = (1..$numPrices+1).roll xx $numPrices ;
 
my $actualMax = getMaxPrice(@prices);
say "Using $numPrices items with prices from 0 to $actualMax:";
 
my @res = getAll5000(@prices, 0, $actualMax, 5000);
say "Split into ", +@res, " bins of approx 5000 elements:";
 
for @res -> @row {
   my ($min,$max,$subtotal) = @row;
   $max = $actualMax if $max > $actualMax ;
   printf "  From %6d to %6d with %4d items\n", $min, $max, $subtotal 
}
 
```

#### Output:
```
Using 99506 items with prices from 0 to 99507:
Split into 20 bins of approx 5000 elements:
  From      0 to   4983 with 5000 items
  From   4984 to  10034 with 5003 items
  From  10035 to  15043 with 4998 items
  From  15044 to  20043 with 5001 items
  From  20044 to  24987 with 5001 items
  From  24988 to  30000 with 4998 items
  From  30001 to  34954 with 5000 items
  From  34955 to  40018 with 5000 items
  From  40019 to  45016 with 5000 items
  From  45017 to  49950 with 5000 items
  From  49951 to  54877 with 4999 items
  From  54878 to  59880 with 5002 items
  From  59881 to  64807 with 5001 items
  From  64808 to  69800 with 5000 items
  From  69801 to  74897 with 5000 items
  From  74898 to  79956 with 5002 items
  From  79957 to  85037 with 5000 items
  From  85038 to  90118 with 5001 items
  From  90119 to  95169 with 5001 items
  From  95170 to  99507 with 4470 items
```
