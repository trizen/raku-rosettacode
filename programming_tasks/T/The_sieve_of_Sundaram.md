[1]: https://rosettacode.org/wiki/The_sieve_of_Sundaram

# [The sieve of Sundaram][1]

```perl
my $nth = 1_000_000;

my $k = Int.new: 2.4 * $nth * log($nth) / 2;

my int @sieve;

@sieve[$k] = 0;

race for (1 .. $k).batch(1000) -> @i {
  for @i -> int $i {
    my int $j = $i;
    while (my int $l = $i + $j + 2 * $i * $j++) < $k {
        @sieve[$l] = 1;
    }
  }
}

@sieve[0] = 1;

say "First 100 Sundaram primes:";
say @sieve.kv.map( { next if $^v; $^k * 2 + 1 } )[^100]».fmt("%4d").batch(10).join: "\n";

say "\nOne millionth:";
my ($count, $index);
for @sieve {
    $count += !$_;
    say $index * 2 + 1 and last if $count == $nth;
    ++$index;
}
```

#### Output:
```
First 100 Sundaram primes:
   3    5    7   11   13   17   19   23   29   31
  37   41   43   47   53   59   61   67   71   73
  79   83   89   97  101  103  107  109  113  127
 131  137  139  149  151  157  163  167  173  179
 181  191  193  197  199  211  223  227  229  233
 239  241  251  257  263  269  271  277  281  283
 293  307  311  313  317  331  337  347  349  353
 359  367  373  379  383  389  397  401  409  419
 421  431  433  439  443  449  457  461  463  467
 479  487  491  499  503  509  521  523  541  547

One millionth:
15485867
```
