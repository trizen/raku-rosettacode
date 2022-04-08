[1]: https://rosettacode.org/wiki/Find_Chess960_starting_position_identifier

# [Find Chess960 starting position identifier][1]

```perl
#!/usr/bin/env raku
# derive a chess960 position's SP-ID
unit sub MAIN($array = "♖♘♗♕♔♗♘♖");
 
# standardize on letters for easier processing
my $ascii = $array.trans("♜♞♝♛♚♖♘♗♕♔" => "RNBQKRNBQK");
 
# (optional error-checking)
if $ascii.chars != 8 {
    die "Illegal position: should have exactly eight pieces\n";
}
 
for «K Q» -> $one {
  if +$ascii.indices($one) != 1 {
    die "Illegal position: should have exactly one $one\n";
  }
}
 
for «B N R» -> $two {
  if +$ascii.indices($two) != 2 {
    die "Illegal position: should have exactly two $two\'s\n";
  }
}
 
if $ascii !~~ /'R' .* 'K' .* 'R'/ {
  die "Illegal position: King not between rooks.";
}
 
if [+]($ascii.indices('B').map(* % 2)) != 1 {
  die "Illegal position: Bishops not on opposite colors.";
}
# (end optional error-checking)
 
# Work backwards through the placement rules.
# King and rooks are forced during placement, so ignore them. 
 
# 1. Figure out which knight combination was used:
my @knights = $ascii
  .subst(/<[QB]>/,'',:g)
  .indices('N');
 
my $knight = combinations(5,2).kv.grep(
    -> $i,@c { @c eq @knights }
  )[0][0]; 
 
# 2. Then which queen position:
my $queen = $ascii
  .subst(/<[B]>/,'',:g)
  .index('Q');
 
# 3. Finally the two bishops:
my @bishops = $ascii.indices('B');
my $dark  = @bishops.grep({     $_ %% 2 })[0] div 2;
my $light = @bishops.grep({ not $_ %% 2 })[0] div 2;
 
my $sp-id = 4*(4*(6*$knight + $queen)+$dark)+$light;
 
# standardize output
my $display = $ascii.trans("RNBQK" => "♖♘♗♕♔");
 
say "$display: $sp-id";
```

#### Output:
```
$ c960spid QNRBBNKR
♕♘♖♗♗♘♔♖: 105
$ c960spid
♖♘♗♕♔♗♘♖: 518
```
