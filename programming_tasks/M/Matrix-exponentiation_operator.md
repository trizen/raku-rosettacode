[1]: http://rosettacode.org/wiki/Matrix-exponentiation_operator

# [Matrix-exponentiation operator][1]

```perl6
subset SqMat of Array where { .elems == all(.[]».elems) }
 
multi infix:<*>(SqMat $a, SqMat $b) {[
    for ^$a -> $r {[
        for ^$b[0] -> $c {
            [+] ($a[$r][] Z* $b[].map: *[$c])
        }
    ]}
]}
 
multi infix:<**> (SqMat $m, Int $n is copy where { $_ >= 0 }) {
    my $tmp = $m;
    my $out = [for ^$m -> $i { [ for ^$m -> $j { +($i == $j) } ] } ];
    loop {
	$out = $out * $tmp if $n +& 1;
	last unless $n +>= 1;
	$tmp = $tmp * $tmp;
    }
 
    $out;
}
 
multi show (SqMat $m) {
    my $size = 1;
    for ^$m X ^$m -> ($i, $j) { $size max= $m[$i][$j].Str.chars; }
    say join "\n", $m».fmt("%{$size}s");
}
 
my @m = [1, 2, 0],
	[0, 3, 1],
	[1, 0, 0];
 
for 0 .. 10 -> $order {
    say "### Order $order";
    show @m ** $order;
}
```


Output:


#### Output:
```
### Order 0
1 0 0
0 1 0
0 0 1
### Order 1
1 2 0
0 3 1
1 0 0
### Order 2
1 8 2
1 9 3
1 2 0
### Order 3
 3 26  8
 4 29  9
 1  8  2
### Order 4
11 84 26
13 95 29
 3 26  8
### Order 5
 37 274  84
 42 311  95
 11  84  26
### Order 6
 121  896  274
 137 1017  311
  37  274   84
### Order 7
 395 2930  896
 448 3325 1017
 121  896  274
### Order 8
 1291  9580  2930
 1465 10871  3325
  395  2930   896
### Order 9
 4221 31322  9580
 4790 35543 10871
 1291  9580  2930
### Order 10
 13801 102408  31322
 15661 116209  35543
  4221  31322   9580
```