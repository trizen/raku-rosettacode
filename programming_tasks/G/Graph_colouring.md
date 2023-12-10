[1]: https://rosettacode.org/wiki/Graph_colouring

# [Graph colouring][1]



```perl
sub GraphNodeColor(@RAW) {
   my %OneMany = my %NodeColor;
   for @RAW { %OneMany{$_[0]}.push: $_[1] ; %OneMany{$_[1]}.push: $_[0] }
   my @ColorPool = "0", "1" … ^+%OneMany.elems; # as string
   my %NodePool  = %OneMany.BagHash; # this DWIM is nice
   if %OneMany<NaN>:exists { %NodePool{$_}:delete for %OneMany<NaN>, NaN } # pending
   while %NodePool.Bool {
      my $color = @ColorPool.shift;
      my %TempPool = %NodePool;
      while (my \n = %TempPool.keys.sort.first) {
         %NodeColor{n} = $color;
         %TempPool{n}:delete;
         %TempPool{$_}:delete for @(%OneMany{n}) ; # skip neighbors as well
         %NodePool{n}:delete;
      }
   }
   if %OneMany<NaN>:exists { # islanders use an existing color
      %NodeColor{$_} = %NodeColor.values.sort.first for @(%OneMany<NaN>)
   }
   return %NodeColor
}

my \DATA = [
   [<0 1>,<1 2>,<2 0>,<3 NaN>,<4 NaN>,<5 NaN>],
   [<1 6>,<1 7>,<1 8>,<2 5>,<2 7>,<2 8>,<3 5>,<3 6>,<3 8>,<4 5>,<4 6>,<4 7>],
   [<1 4>,<1 6>,<1 8>,<3 2>,<3 6>,<3 8>,<5 2>,<5 4>,<5 8>,<7 2>,<7 4>,<7 6>],
   [<1 6>,<7 1>,<8 1>,<5 2>,<2 7>,<2 8>,<3 5>,<6 3>,<3 8>,<4 5>,<4 6>,<4 7>],
];

for DATA {
   say "DATA   : ", $_;
   say "Result : ";
   my %out = GraphNodeColor $_;
   say "$_[0]-$_[1]:\t Color %out{$_[0]} ",$_[1].isNaN??''!!%out{$_[1]} for @$_;
   say "Nodes  : ", %out.keys.elems;
   say "Edges  : ", $_.elems;
   say "Colors : ", %out.values.Set.elems;
}
```

#### Output:
```
DATA   : [(0 1) (1 2) (2 0) (3 NaN) (4 NaN) (5 NaN)]
Result :
0-1:     Color 0 1
1-2:     Color 1 2
2-0:     Color 2 0
3-NaN:   Color 0
4-NaN:   Color 0
5-NaN:   Color 0
Nodes  : 6
Edges  : 6
Colors : 3
DATA   : [(1 6) (1 7) (1 8) (2 5) (2 7) (2 8) (3 5) (3 6) (3 8) (4 5) (4 6) (4 7)]
Result :
1-6:     Color 0 1
1-7:     Color 0 1
1-8:     Color 0 1
2-5:     Color 0 1
2-7:     Color 0 1
2-8:     Color 0 1
3-5:     Color 0 1
3-6:     Color 0 1
3-8:     Color 0 1
4-5:     Color 0 1
4-6:     Color 0 1
4-7:     Color 0 1
Nodes  : 8
Edges  : 12
Colors : 2
DATA   : [(1 4) (1 6) (1 8) (3 2) (3 6) (3 8) (5 2) (5 4) (5 8) (7 2) (7 4) (7 6)]
Result :
1-4:     Color 0 1
1-6:     Color 0 2
1-8:     Color 0 3
3-2:     Color 1 0
3-6:     Color 1 2
3-8:     Color 1 3
5-2:     Color 2 0
5-4:     Color 2 1
5-8:     Color 2 3
7-2:     Color 3 0
7-4:     Color 3 1
7-6:     Color 3 2
Nodes  : 8
Edges  : 12
Colors : 4
DATA   : [(1 6) (7 1) (8 1) (5 2) (2 7) (2 8) (3 5) (6 3) (3 8) (4 5) (4 6) (4 7)]
Result :
1-6:     Color 0 1
7-1:     Color 1 0
8-1:     Color 1 0
5-2:     Color 1 0
2-7:     Color 0 1
2-8:     Color 0 1
3-5:     Color 0 1
6-3:     Color 1 0
3-8:     Color 0 1
4-5:     Color 0 1
4-6:     Color 0 1
4-7:     Color 0 1
Nodes  : 8
Edges  : 12
Colors : 2
```
