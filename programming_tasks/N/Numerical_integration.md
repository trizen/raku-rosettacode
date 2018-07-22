[1]: https://rosettacode.org/wiki/Numerical_integration

# [Numerical integration][1]

```perl
use MONKEY-SEE-NO-EVAL;
 
sub leftrect(&f, $a, $b, $n) {
    my $h = ($b - $a) / $n;
    $h * [+] do f($_) for $a, $a+$h ... $b-$h;
}
 
sub rightrect(&f, $a, $b, $n) {
    my $h = ($b - $a) / $n;
    $h * [+] do f($_) for $a+$h, $a+$h+$h ... $b;
}
 
sub midrect(&f, $a, $b, $n) {
    my $h = ($b - $a) / $n;
    $h * [+] do f($_) for $a+$h/2, $a+$h+$h/2 ... $b-$h/2;
}
 
sub trapez(&f, $a, $b, $n) {
    my $h = ($b - $a) / $n;
    my $partial-sum += f($_) * 2 for $a+$h, $a+$h+$h ... $b-$h;
    $h / 2 * [+] f($a), f($b), $partial-sum;
}
 
sub simpsons(&f, $a, $b, $n) {
    my $h = ($b - $a) / $n;
    my $h2 = $h/2;
    my $sum1 = f($a + $h2);
    my $sum2 = 0;
 
    for $a+$h, *+$h ... $b-$h {
        $sum1 += f($_ + $h2);
        $sum2 += f($_);
    }
    ($h / 6) * (f($a) + f($b) + 4*$sum1 + 2*$sum2);
}
 
sub tryem($f, $a, $b, $n, $exact) {
    say "\n$f\n   in [$a..$b] / $n";
    EVAL "my &f = $f;
    say '              exact result: ', $exact;
    say '     rectangle method left: ', leftrect  &f, $a, $b, $n;
    say '    rectangle method right: ', rightrect &f, $a, $b, $n;
    say '      rectangle method mid: ', midrect   &f, $a, $b, $n;
    say 'composite trapezoidal rule: ', trapez    &f, $a, $b, $n;
    say '   quadratic simpsons rule: ', simpsons  &f, $a, $b, $n;"
}
 
tryem '{ $_ ** 3 }', 0, 1, 100, 0.25;
 
tryem '1 / *', 1, 100, 1000, log(100);
 
tryem '*.self', 0, 5_000, 5_000_000, 12_500_000;
 
tryem '*.self', 0, 6_000, 6_000_000, 18_000_000;
```
```text
{ $_ ** 3 }
   in [0..1] / 100
              exact result: 0.25
     rectangle method left: 0.245025
    rectangle method right: 0.255025
      rectangle method mid: 0.249988
composite trapezoidal rule: 0.250025
   quadratic simpsons rule: 0.25
 
1 / *
   in [1..100] / 1000
              exact result: 4.60517018598809
     rectangle method left: 4.65499105751468
    rectangle method right: 4.55698105751468
      rectangle method mid: 4.60476254867838
composite trapezoidal rule: 4.60598605751468
   quadratic simpsons rule: 4.60517038495714
 
*.self
   in [0..5000] / 5000000
              exact result: 12500000
     rectangle method left: 12499997.5
    rectangle method right: 12500002.5
      rectangle method mid: 12500000
composite trapezoidal rule: 12500000
   quadratic simpsons rule: 12500000
 
*.self
   in [0..6000] / 6000000
              exact result: 18000000
     rectangle method left: 17999997
    rectangle method right: 18000003
      rectangle method mid: 18000000
composite trapezoidal rule: 18000000
   quadratic simpsons rule: 18000000
```


Note that these integrations are done with rationals rather than floats, so should be fairly precise (though of course with so few iterations they are not terribly accurate (except when they are)). Some of the sums do overflow into Num (floating point)--currently rakudo allows 64-bit denominators--but at least all of the interval arithmetic is exact.