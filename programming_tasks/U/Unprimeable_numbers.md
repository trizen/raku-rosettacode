[1]: https://rosettacode.org/wiki/Unprimeable_numbers

# [Unprimeable numbers][1]



```perl
use ntheory:from<Perl5> <is_prime>;
use Lingua::EN::Numbers;

sub is-unprimeable (\n) {
     return False if n.&is_prime;
     my \chrs = n.chars;
     for ^chrs -> \place {
         my \pow = 10**(chrs - place - 1);
         my \this = n.substr(place, 1) × pow;
         ^10 .map: -> \dgt {
             next if this == dgt;
             return False if is_prime(n - this + dgt × pow)
         }
     }
     True
}

my @ups = lazy ^∞ .grep: { .&is-unprimeable };

say "First 35 unprimeables:\n" ~ @ups[^35];

say "\n{ordinal-digit(600, :u)} unprimeable: " ~ comma( @ups[599] ) ~ "\n";

^10 .map: -> \n {
    print "First unprimeable that ends with {n}: " ~
    sprintf "%9s\n", comma (n, *+10 … *).race.first: { .&is-unprimeable }
}
```

#### Output:
```
First 35 unprimeables:
200 204 206 208 320 322 324 325 326 328 510 512 514 515 516 518 530 532 534 535 536 538 620 622 624 625 626 628 840 842 844 845 846 848 890

600ᵗʰ unprimeable: 5,242

First unprimeable that ends with 0:       200
First unprimeable that ends with 1:   595,631
First unprimeable that ends with 2:       322
First unprimeable that ends with 3: 1,203,623
First unprimeable that ends with 4:       204
First unprimeable that ends with 5:       325
First unprimeable that ends with 6:       206
First unprimeable that ends with 7:   872,897
First unprimeable that ends with 8:       208
First unprimeable that ends with 9:   212,159
```
