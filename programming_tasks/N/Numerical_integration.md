[1]: https://rosettacode.org/wiki/Numerical_integration

# [Numerical integration][1]


The addition of `<b>Promise</b>`/`<b>await</b>` allows for concurrent computation, and brings a significant speed-up in running time. Which is not to say that it makes this code fast, but it does make it less slow.



Note that these integrations are done with rationals rather than floats, so should be fairly precise (though of course with so few iterations they are not terribly accurate (except when they are)).  Some of the sums do overflow into `Num` (floating point)--currently Rakudo allows 64-bit denominators--but at least all of the interval arithmetic is exact.

```perl
use MONKEY-SEE-NO-EVAL;

sub leftrect(&f, $a, $b, $n) {
    my $h = ($b - $a) / $n;
    my $end = $b-$h;
    my $sum = 0;
    loop (my $i = $a; $i <= $end; $i += $h) { $sum += f($i) }
    $h * $sum;
}

sub rightrect(&f, $a, $b, $n) {
    my $h = ($b - $a) / $n;
    my $sum = 0;
    loop (my $i = $a+$h; $i <= $b; $i += $h) { $sum += f($i) }
    $h * $sum;
}

sub midrect(&f, $a, $b, $n) {
    my $h = ($b - $a) / $n;
    my $sum = 0;
    my ($start, $end) = $a+$h/2, $b-$h/2;
    loop (my $i = $start; $i <= $end; $i += $h) { $sum += f($i) }
    $h * $sum;
}

sub trapez(&f, $a, $b, $n) {
    my $h = ($b - $a) / $n;
    my $partial-sum = 0;
    my ($start, $end) = $a+$h, $b-$h;
    loop (my $i = $start; $i <= $end; $i += $h) { $partial-sum += f($i) * 2 }
    $h / 2 * ( f($a) + f($b) + $partial-sum );
}

sub simpsons(&f, $a, $b, $n) {
    my $h = ($b - $a) / $n;
    my $h2 = $h/2;
    my ($start, $end) = $a+$h, $b-$h;
    my $sum1 = f($a + $h2);
    my $sum2 = 0;
    loop (my $i = $start; $i <= $end; $i += $h) {
        $sum1 += f($i + $h2);
        $sum2 += f($i);
    }
    ($h / 6) * (f($a) + f($b) + 4*$sum1 + 2*$sum2);
}

sub integrate($f, $a, $b, $n, $exact) {
    my $e = 0.000001;
    my $r0 = "$f\n   in [$a..$b] / $n\n"
        ~ '              exact result: '~ $exact.round($e);

    my ($r1,$r2,$r3,$r4,$r5);
    my &f;
    EVAL "&f = $f";
    my $p1 = Promise.start( { $r1 = '     rectangle method left: '~  leftrect(&f, $a, $b, $n).round($e) } );
    my $p2 = Promise.start( { $r2 = '    rectangle method right: '~ rightrect(&f, $a, $b, $n).round($e) } );
    my $p3 = Promise.start( { $r3 = '      rectangle method mid: '~   midrect(&f, $a, $b, $n).round($e) } );
    my $p4 = Promise.start( { $r4 = 'composite trapezoidal rule: '~    trapez(&f, $a, $b, $n).round($e) } );
    my $p5 = Promise.start( { $r5 = '   quadratic simpsons rule: '~  simpsons(&f, $a, $b, $n).round($e) } );

    await $p1, $p2, $p3, $p4, $p5;
    $r0, $r1, $r2, $r3, $r4, $r5;
}

.say for integrate '{ $_ ** 3 }', 0,     1,       100,       0.25; say '';
.say for integrate '1 / *',       1,   100,      1000,   log(100); say '';
.say for integrate '*.self',      0, 5_000, 5_000_000, 12_500_000; say '';
.say for integrate '*.self',      0, 6_000, 6_000_000, 18_000_000;
```

#### Output:
```
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
              exact result: 4.60517
     rectangle method left: 4.654991
    rectangle method right: 4.556981
      rectangle method mid: 4.604763
composite trapezoidal rule: 4.605986
   quadratic simpsons rule: 4.60517

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
