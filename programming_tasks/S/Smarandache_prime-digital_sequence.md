[1]: https://rosettacode.org/wiki/Smarandache_prime-digital_sequence

# [Smarandache prime-digital sequence][1]



```perl
use Lingua::EN::Numbers;
use ntheory:from<Perl5> <:all>;

# Implemented as a lazy, extendable list
my $spds = grep { .&is_prime }, flat [2,3,5,7], [23,27,33,37,53,57,73,77], -> $p
  { state $o++; my $oom = 10**(1+$o); [ flat (2,3,5,7).map: -> $l { (|$p).map: $l×$oom + * } ] } … *;

say 'Smarandache prime-digitals:';
printf "%22s: %s\n", ordinal(1+$_).tclc, comma $spds[$_] for flat ^25, 99, 999, 9999, 99999;
```

#### Output:
```
Smarandache prime-digitals:
                 First: 2
                Second: 3
                 Third: 5
                Fourth: 7
                 Fifth: 23
                 Sixth: 37
               Seventh: 53
                Eighth: 73
                 Ninth: 223
                 Tenth: 227
              Eleventh: 233
               Twelfth: 257
            Thirteenth: 277
            Fourteenth: 337
             Fifteenth: 353
             Sixteenth: 373
           Seventeenth: 523
            Eighteenth: 557
            Nineteenth: 577
             Twentieth: 727
          Twenty-first: 733
         Twenty-second: 757
          Twenty-third: 773
         Twenty-fourth: 2,237
          Twenty-fifth: 2,273
         One hundredth: 33,223
        One thousandth: 3,273,527
        Ten thousandth: 273,322,727
One hundred thousandth: 23,325,232,253
```
