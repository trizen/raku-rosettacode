[1]: https://rosettacode.org/wiki/Non-transitive_dice

# [Non-transitive dice][1]

Thanks to Thundergnat for the nice "less-is-more" tweaks now the 4 dice portion takes around 10 (down from 17 ) minutes to run ..

```perl
# 20201225 Raku programming solution
 
my @dicepool = ^4 xx 4 ;
 
sub FaceCombs(\N, @dp) { # for brevity, changed to use 0-based dice on input
   my @res = my @found = [];
   for [X] @dp {
      unless @found[ my \key = [+] ( N «**« (N-1…0) ) Z* .sort ] {
         @found[key] = True;
         @res.push: $_ »+» 1
      }
   }
   @res
}
 
sub infix:<⚖️>(@x, @y) { +($_{Less} <=> $_{More}) given (@x X<=> @y).Bag }
 
sub findIntransitive(\N, \cs) {
   my @res = [];
   race for [X] ^+cs xx N -> @lot {
      my $skip = False;
      for @lot.rotor(2 => -1) {
         { $skip = True and last } unless cs[ @_[0] ] ⚖️ cs[ @_[1] ] == -1
      }
      next if $skip;
      if cs[ @lot[0] ] ⚖️ cs[ @lot[*-1] ] == 1 { @res.push: [ cs[ @lot ] ] }
   }
   @res
}
 
say "Number of eligible 4-faced dice : ", +(my \combs = FaceCombs(4,@dicepool));
for 3, 4 {
   my @output = findIntransitive($_, combs);
   say +@output, " ordered lists of $_ non-transitive dice found, namely:";
   .say for @output;
}
```

#### Output:
```
Number of eligible 4-faced dice : 35
3 ordered lists of 3 non-transitive dice found, namely:
[(1 1 4 4) (2 2 2 4) (1 3 3 3)]
[(1 3 3 3) (1 1 4 4) (2 2 2 4)]
[(2 2 2 4) (1 3 3 3) (1 1 4 4)]
4 ordered lists of 4 non-transitive dice found, namely:
[(1 1 4 4) (2 2 2 4) (2 2 3 3) (1 3 3 3)]
[(1 3 3 3) (1 1 4 4) (2 2 2 4) (2 2 3 3)]
[(2 2 2 4) (2 2 3 3) (1 3 3 3) (1 1 4 4)]
[(2 2 3 3) (1 3 3 3) (1 1 4 4) (2 2 2 4)]
```
