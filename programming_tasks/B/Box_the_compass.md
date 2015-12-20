[1]: http://rosettacode.org/wiki/Box_the_compass

# [Box the compass][1]

```perl6
sub point (Int $index) {
    my $ix = $index % 32;
    if $ix +& 1
        { "&point(($ix + 1) +& 28) by &point(((2 - ($ix +& 2)) * 4) + $ix +& 24)" }
    elsif $ix +& 2
        { "&point(($ix + 2) +& 24)-&point(($ix +| 4) +& 28)" }
    elsif $ix +& 4
        { "&point(($ix + 8) +& 16)&point(($ix +| 8) +& 24)" }
    else
        { <north east south west>[$ix div 8]; }
}
 
sub test-angle ($ix) { $ix * 11.25 + (0, 5.62, -5.62)[ $ix % 3 ] }
sub angle-to-point(\𝜽) { floor 𝜽 / 360 * 32 + 0.5 }
 
for 0 .. 32 -> $ix {
    my \𝜽 = test-angle($ix);
    printf "  %2d %6.2f° %s\n",
              $ix % 32 + 1,
                  𝜽,
                         tc point angle-to-point 𝜽;
}
```


Output:


#### Output:
```
   1   0.00° North
   2  16.87° North by east
   3  16.88° North-northeast
   4  33.75° Northeast by north
   5  50.62° Northeast
   6  50.63° Northeast by east
   7  67.50° East-northeast
   8  84.37° East by north
   9  84.38° East
  10 101.25° East by south
  11 118.12° East-southeast
  12 118.13° Southeast by east
  13 135.00° Southeast
  14 151.87° Southeast by south
  15 151.88° South-southeast
  16 168.75° South by east
  17 185.62° South
  18 185.63° South by west
  19 202.50° South-southwest
  20 219.37° Southwest by south
  21 219.38° Southwest
  22 236.25° Southwest by west
  23 253.12° West-southwest
  24 253.13° West by south
  25 270.00° West
  26 286.87° West by north
  27 286.88° West-northwest
  28 303.75° Northwest by west
  29 320.62° Northwest
  30 320.63° Northwest by north
  31 337.50° North-northwest
  32 354.37° North by west
   1 354.38° North
```