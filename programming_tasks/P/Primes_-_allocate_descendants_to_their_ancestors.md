[1]: https://rosettacode.org/wiki/Primes_-_allocate_descendants_to_their_ancestors

# [Primes - allocate descendants to their ancestors][1]



```perl
my $max = 99;
my @primes = (2 .. $max).grep: *.is-prime;
my %tree;
(1..$max).map: {
    %tree{$_}<ancestor> = ();
    %tree{$_}<descendants> = {};
};


sub allocate ($n, $i = 0, $sum = 0, $prod = 1) {
    return if $n < 4;
    for @primes.kv -> $k, $p {
        next if $k < $i;
        if ($sum + $p) <= $n {
            allocate($n, $k, $sum + $p, $prod * $p);
        } else {
            last if $sum == $prod;
            %tree{$sum}<descendants>{$prod} = True;
            last if $prod > $max;
            %tree{$prod}<ancestor> = %tree{$sum}<ancestor> (|) $sum;
            last;
        }
    }
}

# abbreviate long lists to first and last 5 elements
sub abbrev (@d) { @d < 11 ?? @d !! ( @d.head(5), '...', @d.tail(5) ) }

my @print = flat 1 .. 15, 46, 74, $max;

(1 .. $max).map: -> $term {
    allocate($term);

    if $term == any( @print )  # print some representative lines
    {
        my $dn = +%tree{$term}<descendants> // 0;
        my $dl = abbrev(%tree{$term}<descendants>.keys.sort( +*) // ());
        printf "%2d, %2d Ancestors: %-14s %5d Descendants: %s\n",
          $term, %tree{$term}<ancestor>,
          "[{ %tree{$term}<ancestor>.keys.sort: +* }],", $dn, "[$dl]";
    }
}

say "\nTotal descendants: ",
  sum (1..$max).map({ +%tree{$_}<descendants> });
```

#### Output:
```
 1,  0 Ancestors: [],                0 Descendants: []
 2,  0 Ancestors: [],                0 Descendants: []
 3,  0 Ancestors: [],                0 Descendants: []
 4,  0 Ancestors: [],                0 Descendants: []
 5,  0 Ancestors: [],                1 Descendants: [6]
 6,  1 Ancestors: [5],               2 Descendants: [8 9]
 7,  0 Ancestors: [],                2 Descendants: [10 12]
 8,  2 Ancestors: [5 6],             3 Descendants: [15 16 18]
 9,  2 Ancestors: [5 6],             4 Descendants: [14 20 24 27]
10,  1 Ancestors: [7],               5 Descendants: [21 25 30 32 36]
11,  0 Ancestors: [],                5 Descendants: [28 40 45 48 54]
12,  1 Ancestors: [7],               7 Descendants: [35 42 50 60 64 72 81]
13,  0 Ancestors: [],                8 Descendants: [22 56 63 75 80 90 96 108]
14,  3 Ancestors: [5 6 9],          10 Descendants: [33 49 70 84 100 120 128 135 144 162]
15,  3 Ancestors: [5 6 8],          12 Descendants: [26 44 105 112 125 ... 160 180 192 216 243]
46,  3 Ancestors: [7 10 25],       557 Descendants: [129 205 246 493 518 ... 14171760 15116544 15943230 17006112 19131876]
74,  5 Ancestors: [5 6 8 16 39],  6336 Descendants: [213 469 670 793 804 ... 418414128120 446308403328 470715894135 502096953744 564859072962]
99,  1 Ancestors: [17],          38257 Descendants: [194 1869 2225 2670 2848 ... 3904305912313344 4117822641892980 4392344151352512 4941387170271576 5559060566555523]

Total descendants: 546986
```
