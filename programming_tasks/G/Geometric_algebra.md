[1]: https://rosettacode.org/wiki/Geometric_algebra

# [Geometric algebra][1]

Here we write a simplified version of the [Clifford](https://github.com/grondilu/clifford) module. It is very general as it is of infinite dimension and also contains an anti-euclidean basis @ē in addition to the euclidean basis @e.

```perl
unit class MultiVector;
subset UIntHash of MixHash where .keys.all ~~ UInt;
has UIntHash $.blades;
method narrow { $!blades.keys.any > 0 ?? self !!  ($!blades{0} // 0) }
 
multi method new(Real $x) returns MultiVector { self.new: (0 => $x).MixHash }
multi method new(UIntHash $blades) returns MultiVector { self.new: :$blades }
 
multi method new(Str $ where /^^e(\d+)$$/) { self.new: (1 +< (2*$0)).MixHash }
multi method new(Str $ where /^^ē(\d+)$$/) { self.new: (1 +< (2*$0 + 1)).MixHash }
 
our @e is export = map { MultiVector.new: "e$_" }, ^Inf;
our @ē is export = map { MultiVector.new: "ē$_" }, ^Inf;
 
my sub order(UInt:D $i is copy, UInt:D $j) {
    (state %){$i}{$j} //= do {
	my $n = 0;
	repeat {
	    $i +>= 1;
	    $n += [+] ($i +& $j).polymod(2 xx *);
	} until $i == 0;
	$n +& 1 ?? -1 !! 1;
    }
}
 
multi infix:<+>(MultiVector $A, MultiVector $B) returns MultiVector is export {
    return MultiVector.new: ($A.blades.pairs, |$B.blades.pairs).MixHash;
}
multi infix:<+>(Real $s, MultiVector $B) returns MultiVector is export {
    return MultiVector.new: (0 => $s, |$B.blades.pairs).MixHash;
}
multi infix:<+>(MultiVector $A, Real $s) returns MultiVector is export { $s + $A }
 
multi infix:<*>(MultiVector $,  0) is export { 0  }
multi infix:<*>(MultiVector $A, 1) returns MultiVector is export { $A }
multi infix:<*>(MultiVector $A, Real $s) returns MultiVector is export {
    MultiVector.new: $A.blades.pairs.map({Pair.new: .key, $s*.value}).MixHash
}
multi infix:<*>(MultiVector $A, MultiVector $B) returns MultiVector is export {
    MultiVector.new: do for $A.blades -> $a {
	|do for $B.blades -> $b {
	    ($a.key +^ $b.key) => [*]
	    $a.value, $b.value,
	    order($a.key, $b.key),
	    |grep +*, (
		|(1, -1) xx * Z*
		($a.key +& $b.key).polymod(2 xx *)
	    )
	}
    }.MixHash
}
multi infix:<**>(MultiVector $ , 0) returns MultiVector is export { MultiVector.new }
multi infix:<**>(MultiVector $A, 1) returns MultiVector is export { $A }
multi infix:<**>(MultiVector $A, 2) returns MultiVector is export { $A * $A }
multi infix:<**>(MultiVector $A, UInt $n where $n %% 2) returns MultiVector is export { ($A ** ($n div 2)) ** 2 }
multi infix:<**>(MultiVector $A, UInt $n) returns MultiVector is export { $A * ($A ** ($n div 2)) ** 2 }
 
multi infix:<*>(Real $s, MultiVector $A) returns MultiVector is export { $A * $s }
multi infix:</>(MultiVector $A, Real $s) returns MultiVector is export { $A * (1/$s) }
multi prefix:<->(MultiVector $A) returns MultiVector is export { return -1 * $A }
multi infix:<->(MultiVector $A, MultiVector $B) returns MultiVector is export { $A + -$B }
multi infix:<->(MultiVector $A, Real $s) returns MultiVector is export { $A + -$s }
multi infix:<->(Real $s, MultiVector $A) returns MultiVector is export { $s + -$A }
 
multi infix:<==>(MultiVector $A, MultiVector $B) returns Bool is export { $A - $B == 0 }
multi infix:<==>(Real $x, MultiVector $A) returns Bool is export { $A == $x }
multi infix:<==>(MultiVector $A, Real $x) returns Bool is export {
    my $narrowed = $A.narrow;
    $narrowed ~~ Real and $narrowed == $x;
}
```


And here is the code for verifying the solution:

```perl
use MultiVector;
use Test;
 
plan 29;
 
sub infix:<cdot>($x, $y) { ($x*$y + $y*$x)/2 }
 
for ^5 X ^5 -> ($i, $j) {
    my $s = $i == $j ?? 1 !! 0;
    ok @e[$i] cdot @e[$j] == $s, "e$i cdot e$j = $s";
}
sub random {
    [+] map {
        MultiVector.new:
        :blades(($_ => rand.round(.01)).MixHash)
    }, ^32;
}
 
my ($a, $b, $c) = random() xx 3;
 
ok ($a*$b)*$c == $a*($b*$c), 'associativity';
ok $a*($b + $c) == $a*$b + $a*$c, 'left distributivity';
ok ($a + $b)*$c == $a*$c + $b*$c, 'right distributivity';
my @coeff = (.5 - rand) xx 5;
my $v = [+] @coeff Z* @e[^5];
ok ($v**2).narrow ~~ Real, 'contraction';</pre>
```