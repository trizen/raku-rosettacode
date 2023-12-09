[1]: https://rosettacode.org/wiki/Smarandache-Wellin_primes

# [Smarandache-Wellin primes][1]

The first seven Smarandache-Wellin primes are found in a few seconds on my system. The eighth adds over five minutes to the run time.

```perl
use Lingua::EN::Numbers;

my @primes = (^∞).grep: &is-prime;

my @Smarandache-Wellin = [\~] @primes;

sink @Smarandache-Wellin[1500]; # pre-reify for concurrency

sub derived ($n) { my %digits = $n.comb.Bag; (0..9).map({ %digits{$_} // 0 }).join }

sub abbr ($_) { .chars < 41 ?? $_ !! .substr(0,20) ~ '…' ~ .substr(*-20) ~ " ({.chars} digits)" }

say "Smarandache-Wellin primes:";
say ordinal-digit(++$,:u).fmt("%4s") ~ $_ for (^∞).hyper(:4batch).map({
    next unless (my $sw = @Smarandache-Wellin[$_]).is-prime;
    sprintf ": Index: %4d, Last prime: %5d, %s", $_, @primes[$_], $sw.&abbr
})[^8];

say "\nSmarandache-Wellin derived primes:";
say ordinal-digit(++$,:u).fmt("%4s") ~ $_ for (^∞).hyper(:8batch).map({
    next unless (my $sw = @Smarandache-Wellin[$_].&derived).is-prime;
    sprintf ": Index: %4d, %s", $_, $sw
})[^20];
```

#### Output:
```
Smarandache-Wellin primes:
 1ˢᵗ: Index:    0, Last prime:     2, 2
 2ⁿᵈ: Index:    1, Last prime:     3, 23
 3ʳᵈ: Index:    3, Last prime:     7, 2357
 4ᵗʰ: Index:  127, Last prime:   719, 23571113171923293137…73677683691701709719 (355 digits)
 5ᵗʰ: Index:  173, Last prime:  1033, 23571113171923293137…10131019102110311033 (499 digits)
 6ᵗʰ: Index:  341, Last prime:  2297, 23571113171923293137…22732281228722932297 (1171 digits)
 7ᵗʰ: Index:  434, Last prime:  3037, 23571113171923293137…30013011301930233037 (1543 digits)
 8ᵗʰ: Index: 1428, Last prime: 11927, 23571113171923293137…11903119091192311927 (5719 digits)

Smarandache-Wellin derived primes:
 1ˢᵗ: Index:   31, 4194123321127
 2ⁿᵈ: Index:   71, 547233879626521
 3ʳᵈ: Index:   72, 547233979727521
 4ᵗʰ: Index:  133, 13672766322929571043
 5ᵗʰ: Index:  224, 3916856106393739943689
 6ᵗʰ: Index:  302, 462696313560586013558131
 7ᵗʰ: Index:  308, 532727113760586013758133
 8ᵗʰ: Index:  362, 6430314317473636515467149
 9ᵗʰ: Index:  461, 8734722823685889120488197
10ᵗʰ: Index:  489, 9035923128899919621189209
11ᵗʰ: Index:  494, 9036023329699969621389211
12ᵗʰ: Index:  521, 9337023533410210710923191219
13ᵗʰ: Index:  537, 94374237357103109113243102223
14ᵗʰ: Index:  623, 117416265406198131121272110263
15ᵗʰ: Index:  720, 141459282456260193137317129313
16ᵗʰ: Index:  737, 144466284461264224139325131317
17ᵗʰ: Index:  789, 156483290479273277162351153339
18ᵗʰ: Index:  851, 164518312512286294233375158359
19ᵗʰ: Index: 1086, 208614364610327343341589284471
20ᵗʰ: Index: 1187, 229667386663354357356628334581
```
