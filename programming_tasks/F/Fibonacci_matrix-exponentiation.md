[1]: https://rosettacode.org/wiki/Fibonacci_matrix-exponentiation

# [Fibonacci matrix-exponentiation][1]


Following the general approach of Sidef, and relying on Perl for `fibmod` function. Borrowed/adapted routines from [Ramanujan's_constant](https://rosettacode.org/wiki/Ramanujan%27s_constant) task to allow `FatRat` calculations throughout. Does not quite meet task spec, as n^64 results not working yet.

```perl
use Math::Matrix;
use Inline::Perl5;
my $p5 = Inline::Perl5.new();
$p5.use( 'Math::AnyNum' );

constant D = 53;  # set the size of FatRat calcluations

# matrix exponentiation
sub fibonacci ($n) {
    my $M = Math::Matrix.new( [[1,1],[1,0]] );
    ($M ** $n)[0][1]
}

# calculation of 𝑒
sub postfix:<!> (Int $n) { (constant f = 1, |[\×] 1..*)[$n] }
sub 𝑒 (--> FatRat) { sum map { FatRat.new(1,.!) }, ^D }

# calculation of π
sub π (--> FatRat) {
    my ($a, $n, $g, $z, $pi) = 1, 1, sqrt(1/2.FatRat), 0.25;

    for ^5 {
        given [ ($a + $g)/2, sqrt $a × $g ] {
            $z -= (.[0] - $a)**2 × $n;
            $n += $n;
            ($a, $g) = @$_;
            $pi = ($a ** 2 / $z).substr: 0, 2 + D
        }
    }
    $pi.FatRat
}

# square-root: accepts/return FatRat
multi sqrt(FatRat $r --> FatRat) {
    FatRat.new: sqrt($r.nude[0] × 10**(D×2) div $r.nude[1]), 10**D
}

# square-root: accepts/return Int
multi sqrt(Int $n --> Int) {
    my $guess = 10**($n.chars div 2);
    my $iterator = { ( $^x   +   $n div ($^x) ) div 2 };
    my $endpoint = { $^x == $^y|$^z };
    min ($guess, $iterator … $endpoint)[*-1, *-2]
}

# arithmetic-geometric mean: accepts/returns FatRat
sub AG-mean(FatRat $a is copy, FatRat $g is copy --> FatRat) {
    ($a, $g) = ($a + $g)/2, sqrt $a × $g until $a - $g < 10**-D;
    $a
}

# override built-in definitions with 'FatRat' versions
constant 𝑒 = &𝑒();
constant π = &π();

# approximation of natural log, accepts any numeric, returns FatRat
sub log-approx ($x --> FatRat) {
    constant ln2 = 2 * [+] (((1/3).FatRat**(2*$_+1))/(2*$_+1) for 0..D); # 1/3 = (2-1)/(2+1)
    π / (2 × AG-mean(1.FatRat, 2.FatRat**(2-D)/$x)) - D × ln2
}

# power function, with exponent less than zero: accepts/returns FatRat
multi infix:<**> (FatRat $base, FatRat $exp is copy where * <  0 --> FatRat) {
    constant ε = 10**-D;
    my ($low, $high) = 0.FatRat, 1.FatRat;
    my $mid  = $high / 2;
    my $acc  = my $sqr = sqrt($base);
    $exp = -$exp;

    while (abs($mid - $exp) > ε) {
        $sqr = sqrt($sqr);
        if ($mid <= $exp) { $low  = $mid; $acc ×=   $sqr }
        else              { $high = $mid; $acc ×= 1/$sqr }
        $mid = ($low + $high) / 2
    }

    (1/$acc).substr(0, D).FatRat
}

sub binet_approx (Int $n --> FatRat) {
    constant PHI =   sqrt(1.25.FatRat) + 0.5 ;
    constant IHP = -(sqrt(1.25.FatRat) - 0.5);
    $n × log-approx(PHI) - log-approx(PHI - IHP)
}

sub nth_fib_first_k_digits ($n,$k) {
    my $f     = binet_approx($n);
    my $log10 = log-approx(10);
    floor( 𝑒**($f - $log10×(floor($f/$log10 + 1))) × 10**$k)
}

my &nth_fib_last_k_digits =
    $p5.run('sub { my($n,$k) = @_; Math::AnyNum::fibmod($n, 10**$k) }');

# matrix exponentiation is very inefficient, n^64 not feasible
for (16, 32) -> $n {
    my $f = fibonacci(2**$n);
    printf "F(2^$n) = %s ... %s\n", substr($f,0,20), $f % 10**20
}

# this way is much faster, but not yet able to handle 2^64 case
for 16, 32 -> $n {
    my $first20 = nth_fib_first_k_digits(2**$n, 20);
    my $last20  = nth_fib_last_k_digits(2**$n, 20);
    printf "F(2^$n) = %s ... %s\n", $first20, $last20
}
```

#### Output:
```
F(2^16) = 73199214460290552832 ... 97270190955307463227
F(2^32) = 61999319689381859818 ... 39623735538208076347
F(2^16) = 73199214460290552832 ... 97270190955307463227
F(2^32) = 61999319689381859818 ... 39623735538208076347
```
