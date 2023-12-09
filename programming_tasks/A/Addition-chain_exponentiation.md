[1]: https://rosettacode.org/wiki/Addition-chain_exponentiation

# [Addition-chain exponentiation][1]

```perl
# 20230327 Raku programming solution

use Math::Matrix;

class AdditionChains { has ( $.idx, $.pos, @.chains, @.lvl, %.pat ) is rw; 

   method add_chain { 
      # method 1: add chains depth then breadth first until done 
      return gather given self { 
         take my $newchain = .chains[.idx].clone.append: 
            .chains[.idx][*-1] + .chains[.idx][.pos];
	     .chains.append: $newchain;
         .pos == +.chains[.idx]-1 ?? ( .idx += 1 && .pos = 0 ) !! .pos += 1;
      }
   }

   method find_chain(\nexp where nexp > 0) {
   # method 1 interface: search for chain ending with n, adding more as needed
      return ([1],) if nexp == 1;
      my @chn = self.chains.grep: *.[*-1] == nexp // [];
      unless @chn.Bool { 
         repeat { @chn = self.add_chain } until @chn[*-1][*-1] == nexp 
      }
      return @chn
   }

   method knuth_path(\ngoal) {
      # method 2: knuth method, uses memoization to search for a shorter chain
      return [] if ngoal < 1; 
      until self.pat{ngoal}:exists {
         self.lvl[0] = [ gather for self.lvl[0].Array -> \i {
            for self.knuth_path(i).Array -> \j {
               unless self.pat{i + j}:exists {
                  self.pat{i + j} = i and take i + j
               }
            }
         } ] 
      }
      return self.knuth_path(self.pat{ngoal}).append: ngoal
   }

   multi method cpow(\xbase, \chain) {
#  raise xbase by an addition exponentiation chain for what becomes x**chain[-1]
      my ($pows, %products) = 0, 1 => xbase;

      %products{0} = xbase ~~ Math::Matrix 
         ?? Math::Matrix.new([ [ 1 xx xbase.size[1] ] xx xbase.size[0] ]) !! 1;

      for chain.Array -> \i {
         %products{i} = %products{$pows} * %products{i - $pows};
         $pows = i
      }
      return %products{ chain[*-1] }
   }
}

my $chn =  AdditionChains.new( idx    =>      0, pos =>      0, 
                               chains => ([1],), lvl => ([1],), pat => {1=>0} );

say 'First one hundred addition chain lengths:';
.Str.say for ( (1..100).map: { +$chn.find_chain($_)[0] - 1 } ).rotor: 10;

my %chns = (31415, 27182).map: { $_ => $chn.knuth_path: $_ };

say "\nKnuth chains for addition chains of 31415 and 27182:";
say "Exponent: $_\n  Addition Chain: %chns{$_}[0..*-2]" for %chns.keys;
say '1.00002206445416^31415 = ', $chn.cpow(1.00002206445416, %chns{31415});
say '1.00002550055251^27182 = ', $chn.cpow(1.00002550055251, %chns{27182});
say '(1.000025 + 0.000058i)^27182 = ', $chn.cpow: 1.000025+0.000058i, %chns{27182};
say '(1.000022 + 0.000050i)^31415 = ', $chn.cpow: 1.000022+0.000050i, %chns{31415};

my \sq05 = 0.5.sqrt;
my \mat  = Math::Matrix.new( [[sq05,    0, sq05,     0, 0, 0], 
                             [    0, sq05,    0,  sq05, 0, 0], 
                             [    0, sq05,    0, -sq05, 0, 0],
                             [-sq05,    0, sq05,     0, 0, 0], 
                             [    0,    0,    0,     0, 0, 1], 
                             [    0,    0,    0,     0, 1, 0]] );

say 'matrix A ^ 27182 =';
say my $res27182 = $chn.cpow(mat, %chns{27182});
say 'matrix A ^ 31415 =';
say $chn.cpow(mat, %chns{31415});
say '(matrix A ** 27182) ** 31415 =';
say $chn.cpow($res27182, %chns{31415});
```

#### Output:
```
First one hundred addition chain lengths:
0 1 2 2 3 3 4 3 4 4
5 4 5 5 5 4 5 5 6 5
6 6 6 5 6 6 6 6 7 6
7 5 6 6 7 6 7 7 7 6
7 7 7 7 7 7 8 6 7 7
7 7 8 7 8 7 8 8 8 7
8 8 8 6 7 7 8 7 8 8
9 7 8 8 8 8 8 8 9 7
8 8 8 8 8 8 9 8 9 8
9 8 9 9 9 7 8 8 8 8

Knuth chains for addition chains of 31415 and 27182:
Exponent: 27182
  Addition Chain: 1 2 3 5 7 14 21 35 70 140 143 283 566 849 1698 3396 6792 6799 13591
Exponent: 31415
  Addition Chain: 1 2 3 5 7 14 28 56 61 122 244 488 976 1952 3904 7808 15616 15677 31293
1.00002206445416^31415 = 1.9999999998924638
1.00002550055251^27182 = 1.999999999974053
(1.000025 + 0.000058i)^27182 = -0.01128636963542673+1.9730308496660347i
(1.000022 + 0.000050i)^31415 = 0.00016144681325535107+1.9960329014194498i
matrix A ^ 27182 =
  0  0  0  0  0  0
  0  0  0  0  0  0
  0  0  0  0  0  0
  0  0  0  0  0  0
  0  0  0  0  0  1
  0  0  0  0  1  0
matrix A ^ 31415 =
   0  0  0   0  0  0
   0  0  0   0  0  0
   0  0  0  -0  0  0
  -0  0  0   0  0  0
   0  0  0   0  0  1
   0  0  0   0  1  0
(matrix A ** 27182) ** 31415 =
  0  0  0  0  0  0
  0  0  0  0  0  0
  0  0  0  0  0  0
  0  0  0  0  0  0
  0  0  0  0  0  1
  0  0  0  0  1  0
```
