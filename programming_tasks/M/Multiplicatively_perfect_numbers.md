[1]: https://rosettacode.org/wiki/Multiplicatively_perfect_numbers

# [Multiplicatively perfect numbers][1]

```perl
use List::Divvy;
use Lingua::EN::Numbers;

constant @primes = (^∞).grep: &is-prime;
constant @cubes  = @primes.map: *³;

state $cube = 0;
sub is-mpn(Int $n ) {
    return False if $n.is-prime;
    if $n == @cubes[$cube] {
        ++$cube;
        return True
    }
    my $factor = find-factor($n);
    return True if ($factor.is-prime && ( my $div = $n div $factor ).is-prime && ($div != $factor));
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

constant @mpn = lazy 1, |(2..*).grep: &is-mpn;

say 'Multiplicatively perfect numbers less than 500:';
put @mpn.&upto(500).batch(10)».fmt("%3d").join: "\n";

put "\nThere are:";
for 5e2, 5e3, 5e4, 5e5, 5e6 {
   printf  "%8s MPNs less than %9s, %7s semiprimes.\n",
     comma(my $count = +@mpn.&upto($_)), .Int.&comma,
     comma $count + @primes.map(*²).&upto($_) - @cubes.&upto($_) - 1;
}
```

#### Output:
```
Multiplicatively perfect numbers less than 500:
  1   6   8  10  14  15  21  22  26  27
 33  34  35  38  39  46  51  55  57  58
 62  65  69  74  77  82  85  86  87  91
 93  94  95 106 111 115 118 119 122 123
125 129 133 134 141 142 143 145 146 155
158 159 161 166 177 178 183 185 187 194
201 202 203 205 206 209 213 214 215 217
218 219 221 226 235 237 247 249 253 254
259 262 265 267 274 278 287 291 295 298
299 301 302 303 305 309 314 319 321 323
326 327 329 334 335 339 341 343 346 355
358 362 365 371 377 381 382 386 391 393
394 395 398 403 407 411 413 415 417 422
427 437 445 446 447 451 453 454 458 466
469 471 473 478 481 482 485 489 493 497

There are:
     150 MPNs less than       500,     153 semiprimes.
   1,354 MPNs less than     5,000,   1,365 semiprimes.
  12,074 MPNs less than    50,000,  12,110 semiprimes.
 108,223 MPNs less than   500,000, 108,326 semiprimes.
 978,983 MPNs less than 5,000,000, 979,274 semiprimes.
```
