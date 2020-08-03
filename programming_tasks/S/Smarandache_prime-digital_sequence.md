[1]: https://rosettacode.org/wiki/Smarandache_prime-digital_sequence

# [Smarandache prime-digital sequence][1]

```raku
use Lingua::EN::Numbers; # Version 2.4.0 or higher
 
# Implemented as a lazy, extendable list
my $spds = flat 2,3,5,7, (1..*).map: { grep { .is-prime }, [X~] |((2,3,5,7) xx $_), (3,7) };
 
say 'Smarandache prime-digitals:';
printf "%15s: %s\n", ordinal(1+$_).tclc, comma $spds[$_] for flat ^25, 99, 999, 9999;
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
```
