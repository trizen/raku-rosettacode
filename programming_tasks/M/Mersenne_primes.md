[1]: http://rosettacode.org/wiki/Mersenne_primes

# [Mersenne primes][1]

We already have a multitude of tasks that demonstrate **how** to find Mersenne primes; [Prime decomposition](https://rosettacode.org/wiki/Prime_decomposition), [Primality by trial division](https://rosettacode.org/wiki/Primality_by_trial_division), [Trial factoring of a Mersenne number](https://rosettacode.org/wiki/Trial_factoring_of_a_Mersenne_number), [Lucas-Lehmer test](https://rosettacode.org/wiki/Lucas-Lehmer_test), [Miller–Rabin primality_test](https://rosettacode.org/wiki/Miller%E2%80%93Rabin_primality_test), etc. that all have Perl 6 entries. I'm not sure what I could add here that would be useful.



Hmmm.



It doesn't specify to *calculate* them, only to *list* them; why throw away all of the computer **millenia** of processing power that the GIMPS has invested?

```perl
use HTTP::UserAgent;
use Gumbo;
 
my $table = parse-html(HTTP::UserAgent.new.get('https://www.mersenne.org/primes/').content, :TAG<table>);
 
say 'All known Mersenne primes as of ', Date(now);
 
say 'M', ++$, ": 2$_ - 1"
  for $table[1]».[*][0][*].comb(/'exp_lo='\d+/)».subst(/\D/, '',:g)
  .trans([<0123456789>.comb] => [<⁰¹²³⁴⁵⁶⁷⁸⁹>.comb]).words;
 
```

#### Output:
```
All known Mersenne primes as of 2018-01-27
M1: 2² - 1
M2: 2³ - 1
M3: 2⁵ - 1
M4: 2⁷ - 1
M5: 2¹³ - 1
M6: 2¹⁷ - 1
M7: 2¹⁹ - 1
M8: 2³¹ - 1
M9: 2⁶¹ - 1
M10: 2⁸⁹ - 1
M11: 2¹⁰⁷ - 1
M12: 2¹²⁷ - 1
M13: 2⁵²¹ - 1
M14: 2⁶⁰⁷ - 1
M15: 2¹²⁷⁹ - 1
M16: 2²²⁰³ - 1
M17: 2²²⁸¹ - 1
M18: 2³²¹⁷ - 1
M19: 2⁴²⁵³ - 1
M20: 2⁴⁴²³ - 1
M21: 2⁹⁶⁸⁹ - 1
M22: 2⁹⁹⁴¹ - 1
M23: 2¹¹²¹³ - 1
M24: 2¹⁹⁹³⁷ - 1
M25: 2²¹⁷⁰¹ - 1
M26: 2²³²⁰⁹ - 1
M27: 2⁴⁴⁴⁹⁷ - 1
M28: 2⁸⁶²⁴³ - 1
M29: 2¹¹⁰⁵⁰³ - 1
M30: 2¹³²⁰⁴⁹ - 1
M31: 2²¹⁶⁰⁹¹ - 1
M32: 2⁷⁵⁶⁸³⁹ - 1
M33: 2⁸⁵⁹⁴³³ - 1
M34: 2¹²⁵⁷⁷⁸⁷ - 1
M35: 2¹³⁹⁸²⁶⁹ - 1
M36: 2²⁹⁷⁶²²¹ - 1
M37: 2³⁰²¹³⁷⁷ - 1
M38: 2⁶⁹⁷²⁵⁹³ - 1
M39: 2¹³⁴⁶⁶⁹¹⁷ - 1
M40: 2²⁰⁹⁹⁶⁰¹¹ - 1
M41: 2²⁴⁰³⁶⁵⁸³ - 1
M42: 2²⁵⁹⁶⁴⁹⁵¹ - 1
M43: 2³⁰⁴⁰²⁴⁵⁷ - 1
M44: 2³²⁵⁸²⁶⁵⁷ - 1
M45: 2³⁷¹⁵⁶⁶⁶⁷ - 1
M46: 2⁴²⁶⁴³⁸⁰¹ - 1
M47: 2⁴³¹¹²⁶⁰⁹ - 1
M48: 2⁵⁷⁸⁸⁵¹⁶¹ - 1
M49: 2⁷⁴²⁰⁷²⁸¹ - 1
M50: 2⁷⁷²³²⁹¹⁷ - 1
```