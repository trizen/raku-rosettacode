[1]: https://rosettacode.org/wiki/Prime_decomposition

# [Prime decomposition][1]

### Pure Perl 6



This is a pure perl 6 version that uses no outside libraries. It uses a variant of Pollard's rho factoring algorithm and is fairly performent when factoring numbers &lt; 2⁸⁰; typically taking well under a second on an i7. It starts to slow down with larger numbers, but really bogs down factoring numbers that have more than 1 factor larger than about 2⁴⁰.

```raku
sub prime-factors ( Int $n where * > 0 ) {
    return $n if $n.is-prime;
    return [] if $n == 1;
    my $factor = find-factor( $n );
    sort flat prime-factors( $factor ), prime-factors( $n div $factor );
}
 
sub find-factor ( Int $n, $constant = 1 ) {
    my $x      = 2;
    my $rho    = 1;
    my $factor = 1;
    while $factor == 1 {
        $rho *= 2;
        my $fixed = $x;
        for ^$rho {
            $x = ( $x * $x + $constant ) % $n;
            $factor = ( $x - $fixed ) gcd $n;
            last if 1 < $factor;
        }
    }
    $factor = find-factor( $n, $constant + 1 ) if $n == $factor;
    $factor;
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
factors of 536870911: 233 × 2089 × 1103          in 0.003 sec.
factors of 2199023255551: 13367 × 164511353      in 0.009 sec.
factors of 576460752303423487: 179951 × 3203431780337    in 0.020 sec.
factors of 2361183241434822606847: 228479 × 48544121 × 212885833         in 0.185 sec.
factors of 604462909807314587353087: 2687 × 202029703 × 1113491139767    in 0.334 sec.
factors of 158456325028528675187087900671: 11447 × 13842607235828485645766393    in 0.004 sec.
factors of 166153499473114484112975882535043071: 7 × 73 × 79 × 937 × 6553 × 8191 × 86113 × 121369 × 7830118297   in 0.020 sec.
factors of 5465610891074107968111136514192945634873647594456118359804135903459867604844945580205745718497: 165901 × 10424087 × 18830281 × 53204737 × 56402249 × 59663291 × 91931221 × 95174413 × 305293727939 × 444161842339 × 790130065009      in 29.986 sec.
```


There is a Perl 6 module available: Prime::Factor, that uses essentially this algorithm with some minor performance tweaks.



### External library



If you really need a speed boost, load the highly optimized Perl 5 ntheory module. It needs a little extra plumbing to deal with the lack of built-in big integer support, but for large number factoring the interface overhead is worth it.

```raku
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
factors of 536870911: 233 × 1103 × 2089          in 0.001 sec.
factors of 2199023255551: 13367 × 164511353      in 0.001 sec.
factors of 576460752303423487: 179951 × 3203431780337    in 0.001 sec.
factors of 2361183241434822606847: 228479 × 48544121 × 212885833         in 0.012 sec.
factors of 604462909807314587353087: 2687 × 202029703 × 1113491139767    in 0.003 sec.
factors of 158456325028528675187087900671: 11447 × 13842607235828485645766393    in 0.001 sec.
factors of 166153499473114484112975882535043071: 7 × 73 × 79 × 937 × 6553 × 8191 × 86113 × 121369 × 7830118297   in 0.001 sec.
factors of 5465610891074107968111136514192945634873647594456118359804135903459867604844945580205745718497: 165901 × 10424087 × 18830281 × 53204737 × 56402249 × 59663291 × 91931221 × 95174413 × 305293727939 × 444161842339 × 790130065009      in 0.064 sec.
```