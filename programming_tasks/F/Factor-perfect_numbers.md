[1]: https://rosettacode.org/wiki/Factor-perfect_numbers

# [Factor-perfect numbers][1]

```perl
# 20221029 Raku programming solution

sub propdiv (\x) {
   my @l = 1 if x > 1;
   for (2 .. x.sqrt.floor) -> \d {
      unless x % d { @l.push: d; my \y = x div d; @l.push: y if y != d }
   }
   @l
}

sub moreMultiples (@toSeq, @fromSeq) {
   my @oneMores = gather for @fromSeq -> \j {
      take @toSeq.clone.push(j) if j > @toSeq[*-1] and j %% @toSeq[*-1]
   }
   return () unless @oneMores.Bool;
   for 0..^@oneMores {
      @oneMores.append: moreMultiples @oneMores[$_], @fromSeq
   }
   @oneMores
}

sub erdosFactorCount (\n) {
   state %cache;
   my ($sum,@divs) = 0, |(propdiv n)[1..*];
   for @divs -> \d {
      unless %cache{my \t = n div d}:exists { %cache{t} = erdosFactorCount(t) }
      $sum += %cache{t}
   }
   ++$sum
}

my @listing = moreMultiples [1], propdiv(48);
given @listing { $_.map: *.push: 48; $_.push: [1,48] }
say @listing.elems," sequences using first definition:";
for @listing.rotor(4) -> \line { line.map: { printf "%-20s", $_ } ; say() }

my @listing2 = gather for (0..^+@listing) -> \j {
   my @seq = |@listing[j];
   @seq.append: 48 if @seq[*-1] != 48;
   take (1..^@seq).map: { @seq[$_] div @seq[$_-1] }
}
say "\n{@listing2.elems} sequences using second definition:";
for @listing2.rotor(4) -> \line { line.map: { printf "%-20s", $_ } ; say() }

say "\nOEIS A163272:";
my ($n,@fpns) = 4, 0,1;
while (@fpns < 7) { @fpns.push($n) if erdosFactorCount($n) == $n; $n += 4 }
say ~@fpns;
```

#### Output:
```
48 sequences using first definition:
1 2 48              1 24 48             1 3 48              1 16 48             
1 4 48              1 12 48             1 6 48              1 8 48              
1 2 24 48           1 2 16 48           1 2 4 48            1 2 12 48           
1 2 6 48            1 2 8 48            1 2 4 24 48         1 2 4 16 48         
1 2 4 12 48         1 2 4 8 48          1 2 4 12 24 48      1 2 4 8 24 48       
1 2 4 8 16 48       1 2 12 24 48        1 2 6 24 48         1 2 6 12 48         
1 2 6 12 24 48      1 2 8 24 48         1 2 8 16 48         1 3 24 48           
1 3 12 48           1 3 6 48            1 3 12 24 48        1 3 6 24 48         
1 3 6 12 48         1 3 6 12 24 48      1 4 24 48           1 4 16 48           
1 4 12 48           1 4 8 48            1 4 12 24 48        1 4 8 24 48         
1 4 8 16 48         1 12 24 48          1 6 24 48           1 6 12 48           
1 6 12 24 48        1 8 24 48           1 8 16 48           1 48                

48 sequences using second definition:
2 24                24 2                3 16                16 3                
4 12                12 4                6 8                 8 6                 
2 12 2              2 8 3               2 2 12              2 6 4               
2 3 8               2 4 6               2 2 6 2             2 2 4 3             
2 2 3 4             2 2 2 6             2 2 3 2 2           2 2 2 3 2           
2 2 2 2 3           2 6 2 2             2 3 4 2             2 3 2 4             
2 3 2 2 2           2 4 3 2             2 4 2 3             3 8 2               
3 4 4               3 2 8               3 4 2 2             3 2 4 2             
3 2 2 4             3 2 2 2 2           4 6 2               4 4 3               
4 3 4               4 2 6               4 3 2 2             4 2 3 2             
4 2 2 3             12 2 2              6 4 2               6 2 4               
6 2 2 2             8 3 2               8 2 3               48                  

OEIS A163272:
0 1 48 1280 2496 28672 29808
```
