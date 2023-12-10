[1]: https://rosettacode.org/wiki/Self_numbers

# [Self numbers][1]

Translated the low memory version of the Go entry but showed only the first 50 self numbers.  The machine for running this task (a Xeon E3110+8GB memory) is showing its age as, 1) took over 6 minutes to complete the Go entry, 2) not even able to run the other two Go alternative entries and 3) needed over 47 minutes to reach 1e6 iterations here.  Anyway I will try this on an i5 box later to see how it goes.

```perl
# 20201127 Raku programming solution

my ( $st, $count, $i, $pow, $digits, $offset, $lastSelf, $done, @selfs) =
     now,      0,  1,   10,       1,       9,         0, False;

# while ( $count < 1e8 ) {
until $done {
   my $isSelf = True;
   my $sum    = (my $start = max ($i-$offset), 0).comb.sum;
   loop ( my $j = $start; $j < $i; $j+=1 ) {
      if $j+$sum == $i { $isSelf = False and last }
      ($j+1)%10 != 0 ?? ( $sum+=1 ) !! ( $sum = ($j+1).comb.sum ) ;
   }
   if $isSelf {
      $count+=1;
      $lastSelf = $i;
      if $count ≤ 50 {
         @selfs.append: $i;
         if $count == 50 {
            say "The first 50 self numbers are:\n", @selfs;
            $done = True;
         }
      }
   }
   $i+=1;
   if $i % $pow == 0 {
      $pow *= 10;
      $digits+=1 ;
      $offset = $digits * 9
   }
}

# say "The 100 millionth self number is ", $lastSelf;
# say "Took ", now - $st, " seconds."
```

#### Output:
```
The first 50 self numbers are:
[1 3 5 7 9 20 31 42 53 64 75 86 97 108 110 121 132 143 154 165 176 187 198 209 211 222 233 244 255 266 277 288 299 310 312 323 334 345 356 367 378 389 400 411 413 424 435 446 457 468]
```
