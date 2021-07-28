[1]: https://rosettacode.org/wiki/Combinations_with_repetitions/Square_digit_chain

# [Combinations with repetitions/Square digit chain][1]



```perl
use v6;
 
sub endsWithOne($n --> Bool) {
   my $digit;
   my $sum = 0;
   my $nn = $n;
   loop {
      while ($nn > 0) {
         $digit = $nn % 10;
         $sum += $digit²;
         $nn = $nn div 10;
      }
      ($sum == 1) and return True;
      ($sum == 89) and return False;
      $nn = $sum;
      $sum = 0;
   }
}
 
my @ks = (7, 8, 11, 14, 17);
 
for @ks -> $k {
   my @sums is default(0) = 1,0;
   my $s;
   for (1 .. $k) -> $n {
      for ($n*81 ... 1) -> $i {
         for (1 .. 9) -> $j {
            $s = $j²;
            if ($s > $i) { last };
            @sums[$i] += @sums[$i-$s];
         }
      }
   }
   my $count1 = 0;
   for (1 .. $k*81) -> $i { if (endsWithOne($i)) {$count1 += @sums[$i]} }
   my $limit = 10**$k - 1;
   say "For k = $k in the range 1 to $limit";
   say "$count1 numbers produce 1 and ",$limit-$count1," numbers produce 89";
}
```

#### Output:
```
For k = 7 in the range 1 to 9999999
1418853 numbers produce 1 and 8581146 numbers produce 89
For k = 8 in the range 1 to 99999999
14255666 numbers produce 1 and 85744333 numbers produce 89
For k = 11 in the range 1 to 99999999999
15091199356 numbers produce 1 and 84908800643 numbers produce 89
For k = 14 in the range 1 to 99999999999999
13770853279684 numbers produce 1 and 86229146720315 numbers produce 89
For k = 17 in the range 1 to 99999999999999999
12024696404768024 numbers produce 1 and 87975303595231975 numbers produce 89
```
