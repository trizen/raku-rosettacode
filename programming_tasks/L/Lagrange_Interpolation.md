[1]: https://rosettacode.org/wiki/Lagrange_Interpolation

# [Lagrange Interpolation][1]

```perl
# 20231020 Raku programming solution

class Point { has ($.x, $.y) }

sub add(@p1, @p2) { return @p1 <<+>> @p2 } # Add two polynomials.

sub multiply(@p1, @p2) { # Multiply two polynomials.
    my ($m,$n) = (@p1,@p2)>>.elems;
    my @prod   = [ 0 xx $m + $n - 1 ];  
    for ^$m X ^$n -> ($i,$j) { @prod[$i+$j] += @p1[$i] * @p2[$j] }
    return @prod;
}

# Multiply a polynomial by a scalar.
sub scalarMultiply(@poly, $x) { return @poly.map: * × $x }

# Divide a polynomial by a scalar.
sub scalarDivide(@poly, $x) { return scalarMultiply(@poly, 1/$x) }

# rosettacode.org/wiki/Horner%27s_rule_for_polynomial_evaluation#Raku
sub evalPoly(@coefs, $x) { return ([o] map { $_ + $x × * }, @coefs.reverse)(0) }

sub lagrange(@pts) {
   my ($c, @polys) = @pts.elems;
   for ^$c -> $i {
      my @poly = 1;
      for ^$c -> $j {
         next if $i == $j;
         @poly = multiply @poly, [ -@pts[$j].x, 1 ]
      }
      @polys[$i] = scalarDivide @poly, evalPoly(@poly.reverse, @pts[$i].x) 
   }
   my @sum = 0;
   for ^$c -> $i { @sum = add @sum, scalarMultiply @polys[$i], @pts[$i].y }
   return @sum;
}

my @pts = [<1 1>,<2 4>,<3 1>,<4 5>].map: { Point.new: x => .[0], y => .[1] };

say [~] lagrange(@pts).kv.rotor(2).reverse.map: -> ($expo,$coef) {
   "{ '+' if $coef ≥ 0 }$coef" ~ do given $expo { 
      when 0  { " " }
      when 1  { "𝑥 " }
      default { "𝑥^$_ " }
   }
}
```


You may [Attempt This Online!](https://ato.pxeger.com/run?1=jVS9btswEJ66-CkOiYrYsaJYbooWsSW4QIYsAYJOBQTHYCzaYSKJAinZEgJl79ipQ1EgQ_MkfYuOfYI-Qo-kZCtuhsqAQd7Px7vvPvLbD0Hu8sfHpzxbHL3_9epsHhEp4ZKzJIN7uCESupZT2GA5ZQ-qTkfm10DCsDtJXRsm6bCHUYJmuUhw58J43Pd9ZYcK9uFDGEK25pDyqEx4zEgkHQMR51HG0qh8hrMPF7X53yzALy6xmNi2kh54oDNVou87NKKxHDUxk1TwEJceBDCAogArhj5YCRyBC9MRgA5ccAFX6PmE_-jyEZrZ1q2qQwMEFutbt1Poe6ox3E3hUBUaKGOlITZ9Y_iog-S0GiCt8uFa7eWcREQ4un2zvtiSgLHIcfGMTbQ5MUlP8dyfX9EJ-oQztmIh_S98E_oi-ssFuMcqSB8juKRZRuY8pA4Xy-M1u2PH51wkVLwevpMzkUd0hhzOtmXM6IpEOckYT_Y_oqp0Jcp2ydURc04XcqeMbsCngD2ixZqpIRWq1UOoUBM63hF0RYWkve5gI7-ILAVJlqqxTCJYp5HG3DasSaUP5WwJw4x7rgZtMZPTqAUzMN4d1bZ25O0mEr-EFhmwhcr3PPSNtq4GpJE11IQGcKTqUJpRlwjlV-dUnVai1Orynk2tQdjyp_VQ02GDgWUI29N6rmoWJjKPEWrwUtONE--vXto7MmhV0zqgNNiNLDFPa11Tl0l1y8YuuL49HsIJ_r_R6xN460-Neu_Na-IkdH0KBXg-OMEADyjN0sXbNMLBkhKCh-nOcJ27lSN4xkV32Gt6N6j6wtIi5balhFLLYO8eDvoHekjKCr8_P-ETUOnNHjxAyGHJVjQBnYql1WNY36BtAGjYw1_Vtrra-uf7l6eWJ6QLgqQ1nisUb-2skBvzmNZvavO2_gU)
