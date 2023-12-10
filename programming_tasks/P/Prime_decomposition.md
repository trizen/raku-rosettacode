[1]: https://rosettacode.org/wiki/Prime_decomposition

# [Prime decomposition][1]





### Pure Raku



This is a pure Raku version that uses no outside libraries. It uses a variant of Pollard's rho factoring algorithm and is fairly performent when factoring numbers &lt; 2⁸⁰; typically taking well under a second on an i7. It starts to slow down with larger numbers, but really bogs down factoring numbers that have more than 1 factor larger than about 2⁴⁰.

```perl
sub prime-factors ( Int $n where * > 0 ) {
    return $n if $n.is-prime;
    return () if $n == 1;
    my $factor = find-factor( $n );
    sort flat ( $factor, $n div $factor ).map: &prime-factors;
}

sub find-factor ( Int $n, $constant = 1 ) {
    return 2 unless $n +& 1;
    if (my $gcd = $n gcd 6541380665835015) > 1 { # magic number: [*] primes 3 .. 43
        return $gcd if $gcd != $n
    }
    my $x      = 2;
    my $rho    = 1;
    my $factor = 1;
    while $factor == 1 {
        $rho = $rho +< 1;
        my $fixed = $x;
        my int $i = 0;
        while $i < $rho {
            $x = ( $x * $x + $constant ) % $n;
            $factor = ( $x - $fixed ) gcd $n;
            last if 1 < $factor;
            $i = $i + 1;
        }
    }
    $factor = find-factor( $n, $constant + 1 ) if $n == $factor;
    $factor;
}

.put for (2²⁹-1, 2⁴¹-1, 2⁵⁹-1, 2⁷¹-1, 2⁷⁹-1, 2⁹⁷-1, 2¹¹⁷-1, 2²⁴¹-1,
5465610891074107968111136514192945634873647594456118359804135903459867604844945580205745718497)\
.hyper(:1batch).map: -> $n {
    my $start = now;
   "factors of $n: ",
    prime-factors($n).join(' × '), " \t in ", (now - $start).fmt("%0.3f"), " sec."
}
```

#### Output:
```
factors of 536870911:  233 × 1103 × 2089    in  0.004  sec.
factors of 2199023255551:  13367 × 164511353     in  0.011  sec.
factors of 576460752303423487:  179951 × 3203431780337       in  0.023  sec.
factors of 2361183241434822606847:  228479 × 48544121 × 212885833    in  0.190  sec.
factors of 604462909807314587353087:  2687 × 202029703 × 1113491139767       in  0.294  sec.
factors of 158456325028528675187087900671:  11447 × 13842607235828485645766393       in  0.005  sec.
factors of 166153499473114484112975882535043071:  7 × 73 × 79 × 937 × 6553 × 8191 × 86113 × 121369 × 7830118297      in  0.022  sec.
factors of 3533694129556768659166595001485837031654967793751237916243212402585239551:  22000409 × 160619474372352289412737508720216839225805656328990879953332340439     in  0.085  sec.
factors of 5465610891074107968111136514192945634873647594456118359804135903459867604844945580205745718497:  165901 × 10424087 × 18830281 × 53204737 × 56402249 × 59663291 × 91931221 × 95174413 × 305293727939 × 444161842339 × 790130065009     in  28.427  sec.
```


There is a Raku module available: [Prime::Factor](https://modules.raku.org/search/?q=Prime%3A%3AFactor), that uses essentially this algorithm with some minor performance tweaks.



### External library



If you really need a speed boost, load the highly optimized Perl 5 ntheory module. It needs a little extra plumbing to deal with the lack of built-in big integer support, but for large number factoring the interface overhead is worth it.

```perl
use Inline::Perl5;
my $p5 = Inline::Perl5.new();
$p5.use( 'ntheory' );

sub prime-factors ($i) {
    my &primes = $p5.run('sub { map { ntheory::todigitstring $_ } sort {$a <=> $b} ntheory::factor $_[0] }');
    primes("$i");
}

for 2²⁹-1, 2⁴¹-1, 2⁵⁹-1, 2⁷¹-1, 2⁷⁹-1, 2⁹⁷-1, 2¹¹⁷-1,
5465610891074107968111136514192945634873647594456118359804135903459867604844945580205745718497
 ->  $n {
    my $start = now;
    say "factors of $n: ",
    prime-factors($n).join(' × '), " \t in ", (now - $start).fmt("%0.3f"), " sec."
}
```

#### Output:
```
factors of 536870911: 233 × 1103 × 2089     in 0.001 sec.
factors of 2199023255551: 13367 × 164511353      in 0.001 sec.
factors of 576460752303423487: 179951 × 3203431780337    in 0.001 sec.
factors of 2361183241434822606847: 228479 × 48544121 × 212885833     in 0.012 sec.
factors of 604462909807314587353087: 2687 × 202029703 × 1113491139767    in 0.003 sec.
factors of 158456325028528675187087900671: 11447 × 13842607235828485645766393    in 0.001 sec.
factors of 166153499473114484112975882535043071: 7 × 73 × 79 × 937 × 6553 × 8191 × 86113 × 121369 × 7830118297   in 0.001 sec.
factors of 5465610891074107968111136514192945634873647594456118359804135903459867604844945580205745718497: 165901 × 10424087 × 18830281 × 53204737 × 56402249 × 59663291 × 91931221 × 95174413 × 305293727939 × 444161842339 × 790130065009      in 0.064 sec.
```
