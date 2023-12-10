[1]: https://rosettacode.org/wiki/Largest_palindrome_product

# [Largest palindrome product][1]

```perl
use Inline::Perl5;
my $p5 = Inline::Perl5.new();
$p5.use: 'ntheory';
my &divisors = $p5.run('sub { ntheory::divisors $_[0] }');

.say for (2..12).map: {.&lpp};

multi lpp ($oom where {!($_ +& 1)}) { # even number of multiplicand digits
    my $f = +(9 x $oom);
    my $o = $oom / 2;
    my $pal = +(9 x $o ~ 0 x $oom ~ 9 x $o);
    sprintf "Largest palindromic product of two %2d-digit integers: %d × %d = %d", $oom, $pal div $f, $f, $pal
}

multi lpp ($oom where {$_ +& 1}) { # odd number of multiplicand digits
    my $p;
    (+(1 ~ (0 x ($oom - 1))) .. +(9 ~ (9 x ($oom - 1)))).reverse.map({ +($_ ~ .flip) }).map: -> $pal {
        for my @factors = divisors("$pal")».Int.grep({ .chars == $oom }).sort( -* ) {
            next unless $pal div $_ ∈ @factors;
            $p = sprintf("Largest palindromic product of two %2d-digit integers: %d × %d = %d", $oom, $pal div $_, $_, $pal);
            last;
        }
        last if $p;
    }
    $p
}
```

#### Output:
```
Largest palindromic product of two  2-digit integers: 91 × 99 = 9009
Largest palindromic product of two  3-digit integers: 913 × 993 = 906609
Largest palindromic product of two  4-digit integers: 9901 × 9999 = 99000099
Largest palindromic product of two  5-digit integers: 99681 × 99979 = 9966006699
Largest palindromic product of two  6-digit integers: 999001 × 999999 = 999000000999
Largest palindromic product of two  7-digit integers: 9997647 × 9998017 = 99956644665999
Largest palindromic product of two  8-digit integers: 99990001 × 99999999 = 9999000000009999
Largest palindromic product of two  9-digit integers: 999920317 × 999980347 = 999900665566009999
Largest palindromic product of two 10-digit integers: 9999900001 × 9999999999 = 99999000000000099999
Largest palindromic product of two 11-digit integers: 99999943851 × 99999996349 = 9999994020000204999999
Largest palindromic product of two 12-digit integers: 999999000001 × 999999999999 = 999999000000000000999999
```
