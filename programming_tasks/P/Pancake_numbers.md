[1]: https://rosettacode.org/wiki/Pancake_numbers

# [Pancake numbers][1]

### Maximum number of flips only

```perl
# 20201110 Raku programming solution
 
sub pancake(\n) {
   my ($gap,$sum,$adj) = 2, 2, -1;
   while ($sum < n) { $sum += $gap = $gap * 2 - 1 and $adj++ }
   return n + $adj;
}
 
for (1..20).rotor(5) { say [~] @_».&{ sprintf "p(%2d) = %2d ",$_,pancake $_ } }
```

#### Output:
```
p( 1) =  0 p( 2) =  1 p( 3) =  3 p( 4) =  4 p( 5) =  5
p( 6) =  7 p( 7) =  8 p( 8) =  9 p( 9) = 10 p(10) = 11
p(11) = 13 p(12) = 14 p(13) = 15 p(14) = 16 p(15) = 17
p(16) = 18 p(17) = 19 p(18) = 20 p(19) = 21 p(20) = 23
```


### Maximum number of flips plus examples using exhaustive search

```perl
sub pancake(\n) {
   my @goalStack = (my \numStacks = $ = 1)..n ; 
   my %newStacks = my %stacks = @goalStack.Str, 0 ;
   for 1..1000 -> \k { 
      my %nextStacks = (); 
      for %newStacks.keys».split(' ') X 2..n -> (@arr, \pos) {
         given flat @arr[0..^pos].reverse, @arr[pos..*-1] {
            %nextStacks{$_.Str} = k unless %stacks{$_.Str}:exists
         }
      }
      %stacks ,= (%newStacks = %nextStacks);
      my $perms    = %stacks.elems;
      my %inverted = %stacks.antipairs;      # this causes loss on examples 
      my \max_key  = %inverted.keys.max;     # but not critical for our purpose
      $perms == numStacks ?? return %inverted{max_key}, k-1 !! numStacks=$perms
   }
   return '', 0
}
 
say "The maximum number of flips to sort a given number of elements is:";
for 1..9 -> $j { given pancake($j) { say "pancake($j) = $_[1] example: $_[0]" }}
```

#### Output:
```
The maximum number of flips to sort a given number of elements is:
pancake(1) = 0 example: 1
pancake(2) = 1 example: 2 1
pancake(3) = 3 example: 1 3 2
pancake(4) = 4 example: 2 4 1 3
pancake(5) = 5 example: 5 1 3 2 4
pancake(6) = 7 example: 5 3 6 1 4 2
pancake(7) = 8 example: 1 5 3 7 4 2 6
pancake(8) = 9 example: 6 1 8 3 5 7 2 4
pancake(9) = 10 example: 3 6 9 2 5 8 4 7 1
```
