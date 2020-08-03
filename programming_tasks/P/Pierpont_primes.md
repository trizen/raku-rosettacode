[1]: https://rosettacode.org/wiki/Pierpont_primes

# [Pierpont primes][1]

### Finesse version



This finesse version never produces more Pierpont numbers than it needs to
fulfill the requested number of primes. It uses a series of parallel iterators
with additional iterators added as required. No need to speculatively generate
an overabundance. No need to rely on magic numbers. No need to sort them. It
produces exactly what is needed, in order, on demand.

```raku
use ntheory:from<Perl5> <is_prime>;
 
sub pierpont ($kind is copy = 1) {
    fail "Unknown type: $kind. Must be one of 1 (default) or 2" if $kind !== 1|2;
    $kind = -1 if $kind == 2;
    my $po3 = 3;
    my $add-one = 3;
    my @iterators = [1,2,4,8 … *].iterator, [3,9,27 … *].iterator;
    my @head = @iterators».pull-one;
 
    gather {
        loop {
            my $key = @head.pairs.min( *.value ).key;
            my $min = @head[$key];
            @head[$key] = @iterators[$key].pull-one;
 
            take $min + $kind if "{$min + $kind}".&is_prime;
 
            if $min >= $add-one {
                @iterators.push: ([|((2,4,8).map: * * $po3) … *]).iterator;
                $add-one = @head[+@iterators - 1] = @iterators[+@iterators - 1].pull-one;
                $po3 *= 3;
            }
        }
    }
}
 
say "First 50 Pierpont primes of the first kind:\n" ~ pierpont[^50].rotor(10)».fmt('%8d').join: "\n";
 
say "\nFirst 50 Pierpont primes of the second kind:\n" ~ pierpont(2)[^50].rotor(10)».fmt('%8d').join: "\n";
 
say "\n250th Pierpont prime of the first kind: " ~ pierpont[249];
 
say "\n250th Pierpont prime of the second kind: " ~ pierpont(2)[249];
```

#### Output:
```
First 50 Pierpont primes of the first kind:
       2        3        5        7       13       17       19       37       73       97
     109      163      193      257      433      487      577      769     1153     1297
    1459     2593     2917     3457     3889    10369    12289    17497    18433    39367
   52489    65537   139969   147457   209953   331777   472393   629857   746497   786433
  839809   995329  1179649  1492993  1769473  1990657  2654209  5038849  5308417  8503057

First 50 Pierpont primes of the second kind:
       2        3        5        7       11       17       23       31       47       53
      71      107      127      191      383      431      647      863      971     1151
    2591     4373     6143     6911     8191     8747    13121    15551    23327    27647
   62207    73727   131071   139967   165887   294911   314927   442367   472391   497663
  524287   786431   995327  1062881  2519423 10616831 17915903 18874367 25509167 30233087

250th Pierpont prime of the first kind: 62518864539857068333550694039553

250th Pierpont prime of the second kind: 4111131172000956525894875083702271
```


### Generalized Pierpont iterator



Alternately, a version that will generate [generalized Pierpont numbers](https://en.wikipedia.org/wiki/Pierpont_prime#Generalization) for any set of prime integers where at least one of the primes is 2.



(Cut down output as it is exactly the same as the first version for {2,3} +1 and {2,3} -1; leaves room to demo some other options.)

```raku
sub smooth-numbers (*@list) {
    cache my \Smooth := gather {
        my %i = (flat @list) Z=> (Smooth.iterator for ^@list);
        my %n = (flat @list) Z=> 1 xx *;
 
        loop {
            take my $n := %n{*}.min;
 
            for @list -> \k {
                %n{k} = %i{k}.pull-one * k if %n{k} == $n;
            }
        }
    }
}
 
# Testing various smooth numbers
 
for   'OEIS: A092506 - 2 + Fermat primes:',        (2),        1,  6,
    "\nOEIS: A000668 - Mersenne primes:",          (2),       -1, 10,
    "\nOEIS: A005109 - Pierpont primes 1st:",      (2,3),      1, 20,
    "\nOEIS: A005105 - Pierpont primes 2nd:",      (2,3),     -1, 20,
    "\nOEIS: A077497:",                            (2,5),      1, 20,
    "\nOEIS: A077313:",                            (2,5),     -1, 20,
    "\nOEIS: A002200 - (\"Hamming\" primes 1st):", (2,3,5),    1, 20,
    "\nOEIS: A293194 - (\"Hamming\" primes 2nd):", (2,3,5),   -1, 20,
    "\nOEIS: A077498:",                            (2,7),      1, 20,
    "\nOEIS: A077314:",                            (2,7),     -1, 20,
    "\nOEIS: A174144 - (\"Humble\" primes 1st):",  (2,3,5,7),  1, 20,
    "\nOEIS: A299171 - (\"Humble\" primes 2nd):",  (2,3,5,7), -1, 20,
    "\nOEIS: A077499:",                            (2,11),     1, 20,
    "\nOEIS: A077315:",                            (2,11),    -1, 20,
    "\nOEIS: A173236:",                            (2,13),     1, 20,
    "\nOEIS: A173062:",                            (2,13),    -1, 20
 
  -> $title, $primes, $add, $count {
 
      say "$title smooth \{$primes\} {$add > 0 ?? '+' !! '-'} 1 ";
      put smooth-numbers(|$primes).map( * + $add ).grep( *.is-prime )[^$count]
}
```

#### Output:
```
OEIS: A092506 - 2 + Fermat primes: smooth {2} + 1 
2 3 5 17 257 65537

OEIS: A000668 - Mersenne primes: smooth {2} - 1 
3 7 31 127 8191 131071 524287 2147483647 2305843009213693951 618970019642690137449562111

OEIS: A005109 - Pierpont primes 1st: smooth {2 3} + 1 
2 3 5 7 13 17 19 37 73 97 109 163 193 257 433 487 577 769 1153 1297

OEIS: A005105 - Pierpont primes 2nd: smooth {2 3} - 1 
2 3 5 7 11 17 23 31 47 53 71 107 127 191 383 431 647 863 971 1151

OEIS: A077497: smooth {2 5} + 1 
2 3 5 11 17 41 101 251 257 401 641 1601 4001 16001 25601 40961 62501 65537 160001 163841

OEIS: A077313: smooth {2 5} - 1 
3 7 19 31 79 127 199 499 1249 1279 1999 4999 5119 8191 12799 20479 31249 49999 51199 79999

OEIS: A002200 - ("Hamming" primes 1st): smooth {2 3 5} + 1 
2 3 5 7 11 13 17 19 31 37 41 61 73 97 101 109 151 163 181 193

OEIS: A293194 - ("Hamming" primes 2nd): smooth {2 3 5} - 1 
2 3 5 7 11 17 19 23 29 31 47 53 59 71 79 89 107 127 149 179

OEIS: A077498: smooth {2 7} + 1 
2 3 5 17 29 113 197 257 449 1373 3137 50177 65537 114689 268913 470597 614657 1075649 3294173 7340033

OEIS: A077314: smooth {2 7} - 1 
3 7 13 31 97 127 223 1567 3583 4801 6271 8191 19207 25087 33613 76831 131071 401407 524287 917503

OEIS: A174144 - ("Humble" primes 1st): smooth {2 3 5 7} + 1 
2 3 5 7 11 13 17 19 29 31 37 41 43 61 71 73 97 101 109 113

OEIS: A299171 - ("Humble" primes 2nd): smooth {2 3 5 7} - 1 
2 3 5 7 11 13 17 19 23 29 31 41 47 53 59 71 79 83 89 97

OEIS: A077499: smooth {2 11} + 1 
2 3 5 17 23 89 257 353 1409 2663 30977 65537 170369 495617 5767169 23068673 59969537 82458113 453519617 3429742097

OEIS: A077315: smooth {2 11} - 1 
3 7 31 43 127 241 967 5323 8191 117127 131071 524287 7496191 10307263 77948683 253755391 428717761 738197503 1714871047 2147483647

OEIS: A173236: smooth {2 13} + 1 
2 3 5 17 53 257 677 3329 13313 35153 65537 2768897 13631489 2303721473 3489660929 4942652417 11341398017 10859007357953 1594691292233729 31403151600910337

OEIS: A173062: smooth {2 13} - 1 
3 7 31 103 127 337 1663 5407 8191 131071 346111 524287 2970343 3655807 22151167 109051903 617831551 1631461441 2007952543 2147483647
```
