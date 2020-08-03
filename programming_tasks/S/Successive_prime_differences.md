[1]: https://rosettacode.org/wiki/Successive_prime_differences

# [Successive prime differences][1]

### Categorized by Successive



Essentially the code from the [Sexy primes](https://rosettacode.org/wiki/Sexy_primes#Raku) task with minor tweaks.

```perl
use Math::Primesieve;
my $sieve = Math::Primesieve.new;
 
my $max = 1_000_000;
my @primes = $sieve.primes($max);
my $filter = @primes.Set;
my $primes = @primes.categorize: &successive;
 
sub successive ($i) {
    gather {
        take '2' if $filter{$i + 2};
        take '1' if $filter{$i + 1};
        take '2_2' if all($filter{$i «+« (2,4)});
        take '2_4' if all($filter{$i «+« (2,6)});
        take '4_2' if all($filter{$i «+« (4,6)});
        take '6_4_2' if all($filter{$i «+« (6,10,12)}) and
            none($filter{$i «+« (2,4,8)});
    }
}
 
sub comma { $^i.flip.comb(3).join(',').flip }
 
for (2,), (1,), (2,2), (2,4), (4,2), (6,4,2) -> $succ {
    say "## Sets of {1+$succ} successive primes <= { comma $max } with " ~
        "successive differences of { $succ.join: ', ' }";
    my $i = $succ.join: '_';
    for 'First', 0, ' Last', * - 1 -> $where, $ind {
        say "$where group: ", join ', ', [\+] flat $primes{$i}[$ind], |$succ
    }
    say '      Count: ', +$primes{$i}, "\n";
}
```

#### Output:
```
## Sets of 2 successive primes <= 1,000,000 with successive differences of 2
First group: 3, 5
 Last group: 999959, 999961
      Count: 8169

## Sets of 2 successive primes <= 1,000,000 with successive differences of 1
First group: 2, 3
 Last group: 2, 3
      Count: 1

## Sets of 3 successive primes <= 1,000,000 with successive differences of 2, 2
First group: 3, 5, 7
 Last group: 3, 5, 7
      Count: 1

## Sets of 3 successive primes <= 1,000,000 with successive differences of 2, 4
First group: 5, 7, 11
 Last group: 999431, 999433, 999437
      Count: 1393

## Sets of 3 successive primes <= 1,000,000 with successive differences of 4, 2
First group: 7, 11, 13
 Last group: 997807, 997811, 997813
      Count: 1444

## Sets of 4 successive primes <= 1,000,000 with successive differences of 6, 4, 2
First group: 31, 37, 41, 43
 Last group: 997141, 997147, 997151, 997153
      Count: 306
```


### Precomputed Differences

```perl
use Math::Primesieve;
 
constant $max    = 1_000_000;
constant @primes = Math::Primesieve.primes($max);
constant @diffs  = @primes.skip Z- @primes;
 
say "Given all ordered primes <= $max, sets with successive differences of:";
for (2,), (1,), (2,2), (2,4), (4,2), (6,4,2) -> @succ {
    my $size = @succ.elems;
 
    my @group_start_offsets = @diffs.rotor( $size => 1-$size )
                                    .grep(:k, { $_ eqv @succ });
 
    my ($first, $last) = @group_start_offsets[0, *-1]
                         .map: { @primes.skip($_).head($size + 1) };
 
    say sprintf '%10s has %5d sets: %15s … %s',
        @succ.gist, @group_start_offsets.elems, $first, $last;
}
```

#### Output:
```
Given all ordered primes <= 1000000, sets with successive differences of:
       (2) has  8169 sets:             3 5 … 999959 999961
       (1) has     1 sets:             2 3 … 2 3
     (2 2) has     1 sets:           3 5 7 … 3 5 7
     (2 4) has  1393 sets:          5 7 11 … 999431 999433 999437
     (4 2) has  1444 sets:         7 11 13 … 997807 997811 997813
   (6 4 2) has   306 sets:     31 37 41 43 … 997141 997147 997151 997153
```