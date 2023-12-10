[1]: https://rosettacode.org/wiki/Welch%27s_t-test

# [Welch&#039;s t-test][1]





### Integration using Simpson's Rule



Perhaps "inspired by C example" may be more accurate. Gamma subroutine from [Gamma function task](https://rosettacode.org/wiki/Gamma_function#Raku).

```perl
sub Γ(\z) {
    constant g = 9;
    z < .5 ?? π / sin(π × z) / Γ(1 - z) !!
    τ.sqrt × (z + g - 1/2)**(z - 1/2) ×
    exp(-(z + g - 1/2)) ×
    [+] <
        1.000000000000000174663
     5716.400188274341379136
   -14815.30426768413909044
    14291.49277657478554025
    -6348.160217641458813289
     1301.608286058321874105
     -108.1767053514369634679
        2.605696505611755827729
       -0.7423452510201416151527e-2
        0.5384136432509564062961e-7
       -0.4023533141268236372067e-8
    > Z× 1, |map 1/(z + *), 0..*
}

sub p-value (@A, @B) {
    return 1 if @A <= 1 or @B <= 1;

    my $a-mean = @A.sum / @A;
    my $b-mean = @B.sum / @B;
    my $a-variance = @A.map( { ($a-mean - $_)² } ).sum / (@A - 1);
    my $b-variance = @B.map( { ($b-mean - $_)² } ).sum / (@B - 1);
    return 1 unless $a-variance && $b-variance;

    my \Welchs-𝒕-statistic = ($a-mean - $b-mean)/($a-variance/@A + $b-variance/@B).sqrt;

    my $DoF = ($a-variance / @A + $b-variance / @B)² /
              (($a-variance² / (@A³ - @A²)) + ($b-variance² / (@B³ - @B²)));

    my $sa = $DoF / 2 - 1;
    my $x = $DoF / (Welchs-𝒕-statistic² + $DoF);
    my $N = 65355;
    my $h = $x / $N;
    my ( $sum1, $sum2 );

    for ^$N »×» $h -> $i {
        $sum1 += (($i + $h / 2) ** $sa) / (1 - ($i + $h / 2)).sqrt;
        $sum2 +=   $i           ** $sa  / (1 -  $i).sqrt;
    }

    (($h / 6) × ( $x ** $sa / (1 - $x).sqrt + 4 × $sum1 + 2 × $sum2)) /
     ( Γ($sa + 1) × π.sqrt / Γ($sa + 1.5) );
}

# Testing
for (
  [<27.5 21.0 19.0 23.6 17.0 17.9 16.9 20.1 21.9 22.6 23.1 19.6 19.0 21.7 21.4>],
  [<27.1 22.0 20.8 23.4 23.4 23.5 25.8 22.0 24.8 20.2 21.9 22.1 22.9 20.5 24.4>],

  [<17.2 20.9 22.6 18.1 21.7 21.4 23.5 24.2 14.7 21.8>],
  [<21.5 22.8 21.0 23.0 21.6 23.6 22.5 20.7 23.4 21.8 20.7 21.7 21.5 22.5 23.6 21.5 22.5 23.5 21.5 21.8>],

  [<19.8 20.4 19.6 17.8 18.5 18.9 18.3 18.9 19.5 22.0>],
  [<28.2 26.6 20.1 23.3 25.2 22.1 17.7 27.6 20.6 13.7 23.2 17.5 20.6 18.0 23.9 21.6 24.3 20.4 24.0 13.2>],

  [<30.02 29.99 30.11 29.97 30.01 29.99>],
  [<29.89 29.93 29.72 29.98 30.02 29.98>],

  [<3.0 4.0 1.0 2.1>],
  [<490.2 340.0 433.9>]
) -> @left, @right { say p-value @left, @right }
```

#### Output:
```
0.0213780014628669
0.148841696605328
0.0359722710297969
0.0907733242856673
0.010751534033393
```


### Using Burkhardt's 'incomplete beta'



This uses the Soper reduction formula to evaluate the integral, which converges much more quickly than Simpson's formula.

```perl
sub lgamma ( Num(Real) \n --> Num ){
  use NativeCall;
  sub lgamma (num64 --> num64) is native {}
  lgamma( n )
}

sub p-value (@a, @b) {
    return 1 if @a.elems | @b.elems ≤ 1;
    my $mean1 = @a.sum / @a.elems;
    my $mean2 = @b.sum / @b.elems;
    return 1 if $mean1 == $mean2;

    my $variance1 = sum (@a «-» $mean1) X**2;
    my $variance2 = sum (@b «-» $mean2) X**2;
    return 1 if $variance1 | $variance2 == 0;

    $variance1 /= @a.elems - 1;
    $variance2 /= @b.elems - 1;
    my $Welchs-𝒕-statistic = ($mean1-$mean2)/sqrt($variance1/@a.elems+$variance2/@b.elems);
    my $DoF = ($variance1/@a.elems + $variance2/@b.elems)² /
               (($variance1 × $variance1)/(@a.elems × @a.elems × (@a.elems-1)) +
                ($variance2 × $variance2)/(@b.elems × @b.elems × (@b.elems-1))
               );
    my $A     = $DoF / 2;
    my $value = $DoF / ($Welchs-𝒕-statistic² + $DoF);
    return $value if $A | $value ≤ 0 or $value ≥ 1;

    # from here, translation of John Burkhardt's C
    my $beta  = lgamma($A) + 0.57236494292470009 - lgamma($A+0.5); # constant is logΓ(.5), more precise than 'lgamma' routine
    my $eps   = 10**-15;
    my $psq = $A + 0.5;
    my $cx = 1 - $value;
    my ($xx,$pp,$qq,$indx);
    if $A < $psq × $value { ($xx, $cx, $pp, $qq, $indx) = $cx, $value, 0.5,  $A, 1 }
    else                  { ($xx,      $pp, $qq, $indx) =      $value,  $A, 0.5, 0 }
    my $term = my $ai = $value = 1;
    my $ns = floor $qq + $cx × $psq;

    # Soper reduction formula
    my $qq-ai = $qq - $ai;
    my $rx = $ns == 0 ?? $xx !! $xx / $cx;
    loop {
        $term  ×= $qq-ai × $rx / ($pp + $ai);
        $value += $term;
        $qq-ai  = $term.abs;
        if $qq-ai ≤ $eps & $eps×$value {
            $value = $value × ($pp × $xx.log + ($qq - 1) × $cx.log - $beta).exp / $pp;
            $value = 1 - $value if $indx;
            last
        }
        $ai++;
        $ns--;
        if $ns ≥ 0 {
            $qq-ai = $qq - $ai;
            $rx    = $xx if $ns == 0;
        } else {
            $qq-ai = $psq;
            $psq  += 1;
        }
    }
    $value
}

my $error = 0;
my @answers = (
0.021378001462867,
0.148841696605327,
0.0359722710297968,
0.090773324285671,
0.0107515611497845,
0.00339907162713746,
0.52726574965384,
0.545266866977794,
);

for (
    [<27.5 21.0 19.0 23.6 17.0 17.9 16.9 20.1 21.9 22.6 23.1 19.6 19.0 21.7 21.4>],
    [<27.1 22.0 20.8 23.4 23.4 23.5 25.8 22.0 24.8 20.2 21.9 22.1 22.9 20.5 24.4>],

    [<17.2 20.9 22.6 18.1 21.7 21.4 23.5 24.2 14.7 21.8>],
    [<21.5 22.8 21.0 23.0 21.6 23.6 22.5 20.7 23.4 21.8 20.7 21.7 21.5 22.5 23.6 21.5 22.5 23.5 21.5 21.8>],

    [<19.8 20.4 19.6 17.8 18.5 18.9 18.3 18.9 19.5 22.0>],
    [<28.2 26.6 20.1 23.3 25.2 22.1 17.7 27.6 20.6 13.7 23.2 17.5 20.6 18.0 23.9 21.6 24.3 20.4 24.0 13.2>],

    [<30.02 29.99 30.11 29.97 30.01 29.99>],
    [<29.89 29.93 29.72 29.98 30.02 29.98>],

    [<3.0 4.0 1.0 2.1>],
    [<490.2 340.0 433.9>],

    [<0.010268 0.000167 0.000167>],
    [<0.159258 0.136278 0.122389>],

    [<1.0/15 10.0/62.0>],
    [<1.0/10 2/50.0>],

    [<9/23.0 21/45.0 0/38.0>],
    [<0/44.0 42/94.0 0/22.0>],
) -> @left, @right {
    my $p-value = p-value @left, @right;
    printf("p-value = %.14g\n",$p-value);
    $error += abs($p-value - shift @answers);
}
printf("cumulative error is %g\n", $error);
```

#### Output:
```
p-value = 0.021378001462867
p-value = 0.14884169660533
p-value = 0.035972271029797
p-value = 0.090773324285667
p-value = 0.010751561149784
p-value = 0.0033990716271375
p-value = 0.52726574965384
p-value = 0.54526686697779
cumulative error is 5.30131e-15
```
