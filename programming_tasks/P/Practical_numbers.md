[1]: https://rosettacode.org/wiki/Practical_numbers

# [Practical numbers][1]

```perl
use Prime::Factor:ver<0.3.0+>;

sub is-practical ($n) {
   return True  if $n == 1;
   return False if $n % 2;
   my @proper = $n.&proper-divisors :sort;
   return True if all( @proper.rotor(2 => -1).map: { .[0] / .[1] >= .5 } );
   my @proper-sums = @proper.combinations».sum.unique.sort;
   +@proper-sums < $n-1 ?? False !! @proper-sums[^$n] eqv (^$n).list ?? True !! False
}

say "{+$_} matching numbers:\n{.batch(10)».fmt('%3d').join: "\n"}\n"
    given [ (1..333).hyper(:32batch).grep: { is-practical($_) } ];

printf "%5s is practical? %s\n", $_, .&is-practical for 666, 6666, 66666, 672, 720;
```

#### Output:
```
77 matching numbers:
  1   2   4   6   8  12  16  18  20  24
 28  30  32  36  40  42  48  54  56  60
 64  66  72  78  80  84  88  90  96 100
104 108 112 120 126 128 132 140 144 150
156 160 162 168 176 180 192 196 198 200
204 208 210 216 220 224 228 234 240 252
256 260 264 270 272 276 280 288 294 300
304 306 308 312 320 324 330

  666 is practical? True
 6666 is practical? True
66666 is practical? False
  672 is practical? True
  720 is practical? True
```
