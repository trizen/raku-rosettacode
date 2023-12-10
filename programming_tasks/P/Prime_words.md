[1]: https://rosettacode.org/wiki/Prime_words

# [Prime words][1]

Another in a continuing series of tasks that are a minimal variation of previous ones. This is essentially [Smarandache prime-digital sequence](https://rosettacode.org/wiki/Smarandache_prime-digital_sequence) using ords instead of numerical digits.
Sigh.



In an effort to anticipate / head-off a rash of tiny variant tasks, a series of one-liners:

```perl
my @words = 'unixdict.txt'.IO.words».fc;

sub display ($n, @n, $s = "First 20: ") {"$n;\n{$s}{@n.join: ', '}"}

# The task
say 'Number of words whose ords are all prime: ',
    @words.hyper.grep({ .ords.all.is-prime }).&{display +$_, $_, ''};

# Twelve other minor variants
say "\nNumber of words whose ordinal sum is prime: ",
    @words.grep({ .ords.sum.is-prime }).&{display +$_, .head(20)};

say "\nNumber of words whose ords are all prime, and whose ordinal sum is prime: ",
    @words.hyper.grep({ .ords.all.is-prime && .ords.sum.is-prime }).&{display +$_, $_, ''};

say "\n\nInterpreting the words as if they were base 36 numbers:";

say "\nNumber of words whose 'digits' are all prime in base 36: ",
    @words.hyper.grep({ !.contains(/\W/) && all(.comb».parse-base(36)).is-prime }).&{display +$_, $_, ''};

say "\nNumber of words that are prime in base 36: ",
    @words.grep({ !.contains(/\W/) && :36($_).is-prime }).&{display +$_, .head(20)};

say "\nNumber of words whose base 36 digital sum is prime: ",
    @words.grep({ !.contains(/\W/) && .comb».parse-base(36).sum.is-prime }).&{display +$_, .head(20)};

say "\nNumber of words that are prime in base 36, and whose digital sum is prime: ",
    @words.grep({ !.contains(/\W/) && :36($_).is-prime && .comb».parse-base(36).sum.is-prime }).&{display +$_, .head(20)};

say "\nNumber of words that are prime in base 36, whose digits are all prime, and whose digital sum is prime: ",
    @words.hyper.grep({ !.contains(/\W/) && all(.comb».parse-base(36)).is-prime && :36($_).is-prime && .comb».parse-base(36).sum.is-prime }).&{display +$_, $_, ''};

use Base::Any:ver<0.1.2+>;
set-digits('a'..'z');

say "\n\nTests using a custom base 26 where 'a' through 'z' is 0 through 25 and words are case folded:";

say "\nNumber of words whose 'digits' are all prime using a custom base 26: ",
    @words.hyper.grep({ !.contains(/<-alpha>/) && all(.comb».&from-base(26)).is-prime }).&{display +$_, $_, ''};

say "\nNumber of words that are prime using a custom base 26: ",
    @words.grep({ !.contains(/<-alpha>/) && .&from-base(26).is-prime }).&{display +$_, .head(20)};

say "\nNumber of words whose digital sum is prime using a custom base 26: ",
    @words.grep({ !.contains(/<-alpha>/) && .comb».&from-base(26).sum.is-prime }).&{display +$_, .head(20)};

say "\nNumber of words that are prime in a custom base 26 and whose digital sum is prime in that base: ",
    @words.grep({ !.contains(/<-alpha>/) && .&from-base(26).is-prime && .comb».&from-base(26).sum.is-prime }).&{display +$_, .head(20)};

say "\nNumber of words that are prime in custom base 26, whose digits are all prime, and whose digital sum is prime: ",
    @words.hyper.grep({ !.contains(/<-alpha>/) && all(.comb».&from-base(26)).is-prime && .&from-base(26).is-prime && .comb».&from-base(26).sum.is-prime }).&{display +$_, $_, ''};
```

#### Output:
```
Number of words whose ords are all prime: 36;
a, aaa, age, agee, ak, am, ama, e, egg, eke, em, emma, g, ga, gag, gage, gam, game, gamma, ge, gee, gem, gemma, gm, k, keg, m, ma, mae, magma, make, mamma, me, meek, meg, q

Number of words whose ordinal sum is prime: 3778;
First 20: 10th, 9th, a, a's, aau, ababa, abate, abhorred, abject, ablate, aboard, abrade, abroad, absentee, absentia, absolute, absorptive, absurd, abusive, accelerate

Number of words whose ords are all prime, and whose ordinal sum is prime: 12;
a, e, egg, g, gem, k, keg, m, mae, mamma, meg, q


Interpreting the words as if they were base 36 numbers:

Number of words whose 'digits' are all prime in base 36: 18;
2nd, 5th, 7th, b, d, h, j, n, nd, nh, nj, nv, t, tn, tnt, tv, v, vt

Number of words that are prime in base 36: 1106;
First 20: 10th, 1st, 2nd, 5th, 6th, 7th, abandon, abbott, abdomen, ablution, abolish, abort, abrupt, absorb, abstention, abstract, abutted, accept, accident, acid

Number of words whose base 36 digital sum is prime: 4740;
First 20: 10th, 3rd, 7th, aba, abacus, abalone, abase, abater, abelian, abelson, aberrant, abeyant, ablaze, abort, aboveground, abraham, abrasion, abrasive, abreact, abridge

Number of words that are prime in base 36, and whose digital sum is prime: 300;
First 20: 10th, 7th, abort, accident, acid, ad, adorn, adulthood, afterthought, albeit, alvin, armload, around, arragon, arraign, assassin, asteroid, astound, augean, avocation

Number of words that are prime in base 36, whose digits are all prime, and whose digital sum is prime: 8;
7th, b, d, h, j, n, t, v


Tests using a custom base 26 where 'a' through 'z' is 0 through 25 and words are case folded:

Number of words whose 'digits' are all prime using a custom base 26: 30;
c, cdc, cf, crt, ct, d, dc, dr, f, fcc, fl, ft, ftc, h, l, ltd, n, nc, ncr, nd, nh, nrc, r, rd, t, tn, tnt, ttl, tx, x

Number of words that are prime using a custom base 26: 987;
First 20: abhorrent, abolish, abreact, absurd, ac, act, actual, actuarial, ad, adjutant, adult, advisor, aerosol, aft, agent, agricultural, ah, aid, ajar, al

Number of words whose digital sum is prime using a custom base 26: 5473;
First 20: ababa, aback, abacus, abalone, abase, abater, abc, abdicate, abdomen, abe, abelian, abelson, aberrant, abeyant, ablaze, abolish, abominate, aborigine, aboveground, abraham

Number of words that are prime in a custom base 26 and whose digital sum is prime in that base: 292;
First 20: abolish, abreact, absurd, ac, ad, adjutant, adult, agricultural, ah, aid, al, allah, allied, altar, an, annal, ar, arclength, argonaut, asocial

Number of words that are prime in custom base 26, whose digits are all prime, and whose digital sum is prime: 9;
c, d, f, h, l, n, r, t, x
```
