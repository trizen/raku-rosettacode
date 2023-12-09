[1]: https://rosettacode.org/wiki/Blum_integer

# [Blum integer][1]

```perl
use List::Divvy;
use Lingua::EN::Numbers;

sub is-blum(Int $n ) {
    return False if $n.is-prime;
    my $factor = find-factor($n);
    return True if ($factor.is-prime && ( my $div = $n div $factor ).is-prime && ($div != $factor)
    && ($factor % 4 == 3) && ($div % 4 == 3));
    False;
}

sub find-factor ( Int $n, $constant = 1 ) {
    my $x      = 2;
    my $rho    = 1;
    my $factor = 1;
    while $factor == 1 {
        $rho *= 2;
        my $fixed = $x;
        for ^$rho {
            $x = ( $x * $x + $constant ) % $n;
            $factor = ( $x - $fixed ) gcd $n;
            last if 1 < $factor;
        }
    }
    $factor = find-factor( $n, $constant + 1 ) if $n == $factor;
    $factor;
}

my @blum = lazy (2..Inf).hyper(:1000batch).grep: &is-blum;
say "The first fifty Blum integers:\n" ~
  @blum[^50].batch(10)».fmt("%3d").join: "\n";
say '';

printf "The %9s Blum integer: %9s\n", .&ordinal-digit(:c), comma @blum[$_-1] for 26828, 1e5, 2e5, 3e5, 4e5;

say "\nBreakdown by last digit:";
printf "%d => %%%7.4f:\n", .key, +.value / 4e3 for sort @blum[^4e5].categorize: {.substr(*-1)}
```

#### Output:
```
The first fifty Blum integers:
 21  33  57  69  77  93 129 133 141 161
177 201 209 213 217 237 249 253 301 309
321 329 341 381 393 413 417 437 453 469
473 489 497 501 517 537 553 573 581 589
597 633 649 669 681 713 717 721 737 749

The  26,828th Blum integer:   524,273
The 100,000th Blum integer: 2,075,217
The 200,000th Blum integer: 4,275,533
The 300,000th Blum integer: 6,521,629
The 400,000th Blum integer: 8,802,377

Breakdown by last digit:
1 => %25.0013:
3 => %25.0168:
7 => %24.9973:
9 => %24.9848:
```
