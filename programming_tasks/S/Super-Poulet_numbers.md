[1]: https://rosettacode.org/wiki/Super-Poulet_numbers

# [Super-Poulet numbers][1]

```perl
use Prime::Factor;
use Lingua::EN::Numbers;

my @poulet = lazy (2..*).hyper(:2000batch).grep: { !.is-prime && (1 == expmod 2, $_ - 1, $_) }
my @super-poulet = @poulet.grep: { all .&divisors.skip(1).map: { 2 == expmod 2, $_, $_ } }

say "First 20 super-Poulet numbers:\n" ~ @super-poulet[^20].gist;

for 1e6.Int, 1e7.Int -> $threshold {
    say "\nIndex and value of first super-Poulet greater than {$threshold.&cardinal}:";
    my $index = @super-poulet.first: * > $threshold, :k;
    say "{(1+$index).&ordinal-digit} super-Poulet number == " ~ @super-poulet[$index].&comma;
}
```

#### Output:
```
First 20 super-Poulet numbers:
(341 1387 2047 2701 3277 4033 4369 4681 5461 7957 8321 10261 13747 14491 15709 18721 19951 23377 31417 31609)

Index and value of first super-Poulet greater than one million:
109th super-Poulet number == 1,016,801

Index and value of first super-Poulet greater than ten million:
317th super-Poulet number == 10,031,653
```
