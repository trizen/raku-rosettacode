[1]: http://rosettacode.org/wiki/Geometric_algebra

# [Geometric algebra][1]

```perl
unit class MultiVector;
has Real %.blades{UInt};
method clean { for %!blades { %!blades{.key} :delete unless .value; } }
method narrow {
    for %!blades { return self if .key > 0 && .value !== 0; }
    return %!blades{0} // 0;
}
 
sub e(UInt $n?) returns MultiVector is export {
    $n.defined ?? MultiVector.new(:blades(my Real %{UInt} = (1 +< $n) => 1)) !! MultiVector.new
}
 
my sub order(UInt:D $i is copy, UInt:D $j) is cached {
    my $n = 0;
    repeat {
	$i +>= 1;
	$n += [+] ($i +& $j).base(2).comb;
    } until $i == 0;
    return $n +& 1 ?? -1 !! 1;
}
 
multi infix:<+>(MultiVector $A, MultiVector $B) returns MultiVector is export {
    my Real %blades{UInt} = $A.blades.clone;
    for $B.blades {
	%blades{.key} += .value;
	%blades{.key} :delete unless %blades{.key};
    }
    return MultiVector.new: :%blades;
}
multi infix:<+>(Real $s, MultiVector $A) returns MultiVector is export {
    my Real %blades{UInt} = $A.blades.clone;
    %blades{0} += $s;
    %blades{0} :delete unless %blades{0};
    return MultiVector.new: :%blades;
}
multi infix:<+>(MultiVector $A, Real $s) returns MultiVector is export { $s + $A }
multi infix:<*>(MultiVector $A, MultiVector $B) returns MultiVector is export {
    my Real %blades{UInt};
    for $A.blades -> $a {
	for $B.blades -> $b {
	    my $c = $a.key +^ $b.key;
	    %blades{$c} += $a.value * $b.value * order($a.key, $b.key);
	    %blades{$c} :delete unless %blades{$c};
	}
    }
    return MultiVector.new: :%blades;
}
multi infix:<**>(MultiVector $ , 0) returns MultiVector is export { MultiVector.new }
multi infix:<**>(MultiVector $A, 1) returns MultiVector is export { $A }
multi infix:<**>(MultiVector $A, 2) returns MultiVector is export { $A * $A }
multi infix:<**>(MultiVector $A, UInt $n where $n %% 2) returns MultiVector is export { ($A ** ($n div 2)) ** 2 }
multi infix:<**>(MultiVector $A, UInt $n) returns MultiVector is export { $A * ($A ** ($n div 2)) ** 2 }
 
multi infix:<*>(MultiVector $,  0) returns MultiVector is export { MultiVector.new }
multi infix:<*>(MultiVector $A, 1) returns MultiVector is export { $A }
multi infix:<*>(MultiVector $A, Real $s) returns MultiVector is export {
    return MultiVector.new: :blades(my Real %{UInt} = map { .key => $s * .value }, $A.blades);
}
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
    ok e($i) cdot e($j) == $s, "e$i cdot e$j = $s";
}
sub random {
    [+] map {
        MultiVector.new:
        :blades(my Real %{UInt} = $_ => rand.round(.01))
    }, ^32;
}
 
my ($a, $b, $c) = random() xx 3;
 
ok ($a*$b)*$c == $a*($b*$c), 'associativity';
ok $a*($b + $c) == $a*$b + $a*$c, 'left distributivity';
ok ($a + $b)*$c == $a*$c + $b*$c, 'right distributivity';
my @coeff = (.5 - rand) xx 5;
my $v = [+] @coeff Z* map &e, ^5;
ok ($v**2).narrow ~~ Real, 'contraction';
```

#### Output:
```
1..29
ok 1 - e0 cdot e0 = 1
ok 2 - e0 cdot e1 = 0
ok 3 - e0 cdot e2 = 0
ok 4 - e0 cdot e3 = 0
ok 5 - e0 cdot e4 = 0
ok 6 - e1 cdot e0 = 0
ok 7 - e1 cdot e1 = 1
ok 8 - e1 cdot e2 = 0
ok 9 - e1 cdot e3 = 0
ok 10 - e1 cdot e4 = 0
ok 11 - e2 cdot e0 = 0
ok 12 - e2 cdot e1 = 0
ok 13 - e2 cdot e2 = 1
ok 14 - e2 cdot e3 = 0
ok 15 - e2 cdot e4 = 0
ok 16 - e3 cdot e0 = 0
ok 17 - e3 cdot e1 = 0
ok 18 - e3 cdot e2 = 0
ok 19 - e3 cdot e3 = 1
ok 20 - e3 cdot e4 = 0
ok 21 - e4 cdot e0 = 0
ok 22 - e4 cdot e1 = 0
ok 23 - e4 cdot e2 = 0
ok 24 - e4 cdot e3 = 0
ok 25 - e4 cdot e4 = 1
ok 26 - associativity
ok 27 - left distributivity
ok 28 - right distributivity
ok 29 - contraction
```