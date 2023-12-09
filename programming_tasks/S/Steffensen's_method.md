[1]: https://rosettacode.org/wiki/Steffensen%27s_method

# [Steffensen&#039;s method][1]

```perl
# 20230928 Raku programming solution

sub aitken($f, $p0) {
   my $p2   = $f( my $p1 = $f($p0) ); 
   my $p1m0 = $p1 - $p0;
   return $p0 - $p1m0*$p1m0/($p2-2.0*$p1+$p0);
}

sub steffensenAitken($f, $pinit, $tol, $maxiter) {
   my ($iter, $p) = 1, aitken($f, my $p0 = $pinit);
   while abs($p-$p0) > $tol and $iter < $maxiter {
      $p = aitken($f, $p0 = $p);
      $iter++
   }
   return abs($p-$p0) > $tol ?? NaN !! $p
}

sub deCasteljau($c0, $c1, $c2, $t) {
   my $s = 1.0 - $t;
   return $s*($s*$c0 + $t*$c1) + $t*($s*$c1 + $t*$c2)
}

sub xConvexLeftParabola($t) { return deCasteljau(2.0, -8.0, 2.0, $t) }

sub yConvexRightParabola($t) { return deCasteljau(1.0, 2.0, 3.0, $t) }

sub implicitEquation($x, $y) { return 5.0*$x*$x + $y - 5.0 }

sub f($t) {
   implicitEquation(xConvexLeftParabola($t), yConvexRightParabola($t)) + $t
}

my $t0 = 0.0;
for ^11 {
   print "t0 = {$t0.fmt: '%0.1f'} : ";
   my $t = steffensenAitken(&f, $t0, 0.00000001, 1000);
   if $t.isNaN {
      say "no answer";
   } else {
      my ($x, $y) = xConvexLeftParabola($t), yConvexRightParabola($t);
      if abs(implicitEquation($x, $y)) <= 0.000001 {
         printf "intersection at (%f, %f)\n", $x, $y;
      } else {
         say "spurious solution";
      }
   }
   $t0 += 0.1;
}
```


You may [Attempt This Online!](https://ato.pxeger.com/run?1=jVTNbtswDL7slKdgDWe1m9izPAwommbFUOw2FMXOwwAllVat_skseXVQ5El26WF7mT1Cn2akFHluOwMzElkUyY_8SFo_fjb8pr2__9UamRw_vPit2xVwZW5EFYVyDuEmi-FuAgDlFoUcN0sIZeRE5gRrFC-gN2NlRhrUJ4SwIEUjTNtUJNpDNDmy6yt0z5M8teKMoBaT3WRCeWgjpBSVFtW7YUaqUgbfpi5wLXmnjGj-JhmFJJNdjDmw-ZCNTc6lRiCxTez2WhUC-EpjIoml8taCA6-uwILBaR_HhcEn3CDM40JZXIdJBmQ-m5G0G_D_R5yzM7jgF3BwgO6e-pU450i_-MrbKFxniL9mtOREfNARTRxTW1LzqMz6KMI_usIMVbhhsdu5Y-aP89iH7M7r6rvoPghpLnnDV3XBIxvLYw5zwn7NITmm1W7JcI-zdTgf1Zfr_wBiPcTrJziq3BRqrcz7by03qsYyd6jfDnDe0NB0-CMyW6wBHnhvGfV1egY0wnQ-mrorHRWKam6o01mKYy3rBj4z5sJsGlUZCKz2Do1SWZoTOJxmKZOHOziBYOG7ZtDk2XC_pCkyWAOEdg92nOHLjZSSqE2VplHxQ6j5FoKqxknVt6Jx-DsQhRa9if0i9oVbjvV4nLmfZoxOkzvWkxhOlz5v1sf2RZEQ4CoaLdbkBNxANEW2Uxl_qgIEsCA-1BMCnqbetI2qWw26LlqCCXqH_huj1swoEYZ3iLvS9jebv-H-AA)
