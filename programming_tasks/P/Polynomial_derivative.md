[1]: https://rosettacode.org/wiki/Polynomial_derivative

# [Polynomial derivative][1]

```perl
use Lingua::EN::Numbers:ver<2.8+>;
 
sub pretty (@poly) {
    join( '+', (^@poly).reverse.map: { @poly[$_] ~ "x{.&super}" } )\
    .subst(/['+'|'-']'0x'<[⁰¹²³⁴⁵⁶⁷⁸⁹]>*/, '', :g).subst(/'x¹'<?before <-[⁰¹²³⁴⁵⁶⁷⁸⁹]>>/, 'x')\
    .subst(/'x⁰'$/, '').subst(/'+-'/, '-', :g).subst(/(['+'|'-'|^])'1x'/, {"$0x"}, :g) || 0
}
 
for [5], [4,-3], [-1,3,-2,1], [-1,6,5], [1,1,0,-1,-1] -> $test {
   say "Polynomial: " ~ "[{$test.join: ','}] ➡ " ~ pretty $test;
   my @poly = |$test;
   (^@poly).map: { @poly[$_] *= $_ };
   shift @poly;
   say "Derivative: " ~ "[{@poly.join: ','}] ➡ " ~ pretty @poly;
   say '';
}
```

#### Output:
```
Polynomial: [5] ➡ 5
Derivative: [] ➡ 0

Polynomial: [4,-3] ➡ -3x+4
Derivative: [-3] ➡ -3

Polynomial: [-1,3,-2,1] ➡ x³-2x²+3x-1
Derivative: [3,-4,3] ➡ 3x²-4x+3

Polynomial: [-1,6,5] ➡ 5x²+6x-1
Derivative: [6,10] ➡ 10x+6

Polynomial: [1,1,0,-1,-1] ➡ -x⁴-x³+x+1
Derivative: [1,0,-3,-4] ➡ -4x³-3x²+1
```
