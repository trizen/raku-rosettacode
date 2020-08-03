[1]: https://rosettacode.org/wiki/Pattern_matching

# [Pattern matching][1]

Perl 6 doesn't have algebraic data types (yet), but it does have pretty good pattern matching in multi signatures.

```perl
enum RedBlack <R B>;
 
multi balance(B,[R,[R,$a,$x,$b],$y,$c],$z,$d) { [R,[B,$a,$x,$b],$y,[B,$c,$z,$d]] }
multi balance(B,[R,$a,$x,[R,$b,$y,$c]],$z,$d) { [R,[B,$a,$x,$b],$y,[B,$c,$z,$d]] }
multi balance(B,$a,$x,[R,[R,$b,$y,$c],$z,$d]) { [R,[B,$a,$x,$b],$y,[B,$c,$z,$d]] }
multi balance(B,$a,$x,[R,$b,$y,[R,$c,$z,$d]]) { [R,[B,$a,$x,$b],$y,[B,$c,$z,$d]] }
 
multi balance($col, $a, $x, $b) { [$col, $a, $x, $b] }
 
multi ins( $x, @s [$col, $a, $y, $b] ) {
    when $x before $y     { balance $col, ins($x, $a), $y, $b }
    when $x after $y      { balance $col, $a, $y, ins($x, $b) }
    default               { @s }
}
multi ins( $x, Any:U ) { [R, Any, $x, Any] }
 
multi insert( $x, $s ) {
    [B, |ins($x,$s)[1..3]];
}
 
sub MAIN {
    my $t = Any;
    $t = insert($_, $t) for (1..10).pick(*);
    say $t.gist;
}
```


This code uses generic comparison operators `before` and `after`, so it should work on any ordered type.


#### Output:
```
[B [B [B (Any) 1 [R (Any) 2 (Any)]] 3 [B (Any) 4 [R (Any) 5 (Any)]]] 6 [B [B (Any) 7 (Any)] 8 [B [R (Any) 9 (Any)] 10 (Any)]]]
```