[1]: https://rosettacode.org/wiki/Primes_-_allocate_descendants_to_their_ancestors

# [Primes - allocate descendants to their ancestors][1]

```perl
my $max = 99;
my @primes = (2 .. $max).grep: *.is-prime;
my %tree;
 
sub allocate ($n, $i = 0, $sum = 0, $prod = 1) {
    for @primes.kv -> $k, $p {
        next if $k < $i;
        if ($sum + $p) <= $max {
            allocate($n, $k, $sum + $p, $prod * $p);
        } else {
            last if $sum == $prod;
            %tree{$sum}<descendants>{$prod} = 1;
            %tree{$n}<ancestor> = () unless %tree{$n}<ancestor>;
            %tree{$prod}<ancestor> = ($sum).Set (|) %tree{$sum}<ancestor> unless $prod > $max;
            last;
        }
    }
}
 
sub abbrev (@d) { # abbreviate long lists to first and last 5 elements
    return @d if @d < 11;
    @d[0 .. 4], '...', @d[*-5 .. *-1];
}
 
.&allocate for 1 .. $max;
 
my $total = [+] (1..$max).map({ %tree{$_}<descendants>.keys });
 
for flat 1 .. 15, 46, 99 { # print some representative lines
    printf "%2d, %2d Ancestors: %-15s", $_, %tree{$_}<ancestor>,
        "[{ %tree{$_}<ancestor>.keys.sort: +* }],";
    my $dn = 0; my $dl = '';
    if (%tree{$_}<descendants> !eqv Any) {
        $dn = %tree{$_}<descendants>.keys;
        $dl = abbrev(%tree{$_}<descendants>.keys.sort: +*);
    }
    printf "%4d Descendants: %s", $dn, "[$dl]\n";
}
 
say "Total descendants: $total";
```

#### Output:
```
 1,  0 Ancestors: [],               0 Descendants: []
 2,  0 Ancestors: [],               0 Descendants: []
 3,  0 Ancestors: [],               0 Descendants: []
 4,  0 Ancestors: [],               0 Descendants: []
 5,  0 Ancestors: [],               1 Descendants: [6]
 6,  1 Ancestors: [5],              2 Descendants: [8 9]
 7,  0 Ancestors: [],               2 Descendants: [10 12]
 8,  2 Ancestors: [5 6],            3 Descendants: [15 16 18]
 9,  2 Ancestors: [5 6],            4 Descendants: [14 20 24 27]
10,  1 Ancestors: [7],              5 Descendants: [21 25 30 32 36]
11,  0 Ancestors: [],               5 Descendants: [28 40 45 48 54]
12,  1 Ancestors: [7],              7 Descendants: [35 42 50 60 64 72 81]
13,  0 Ancestors: [],               8 Descendants: [22 56 63 75 80 90 96 108]
14,  3 Ancestors: [5 6 9],         10 Descendants: [33 49 70 84 100 120 128 135 144 162]
15,  3 Ancestors: [5 6 8],         12 Descendants: [26 44 105 112 125 ... 160 180 192 216 243]
46,  3 Ancestors: [7 10 25],      557 Descendants: [129 205 246 493 518 ... 14171760 15116544 15943230 17006112 19131876]
99,  1 Ancestors: [17],         38257 Descendants: [194 1869 2225 2670 2848 ... 3904305912313344 4117822641892980 4392344151352512 4941387170271576 5559060566555523]
Total descendants: 546986
```