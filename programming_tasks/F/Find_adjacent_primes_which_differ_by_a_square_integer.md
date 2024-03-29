[1]: https://rosettacode.org/wiki/Find_adjacent_primes_which_differ_by_a_square_integer

# [Find adjacent primes which differ by a square integer][1]

```perl
use Lingua::EN::Numbers;
use Math::Primesieve;

my $iterator = Math::Primesieve::iterator.new;
my $limit    = 1e10;
my @squares  = (1..30).map: *²;
my $last     = 2;
my @gaps;
my @counts;

loop {
    my $this = (my $p = $iterator.next) - $last;
    quietly @gaps[$this].push($last) if +@gaps[$this] < 10;
    @counts[$this]++;
    last if $p > $limit;
    $last = $p;
}

print "Adjacent primes up to {comma $limit.Int} with a gap value that is a perfect square:";
for @gaps.pairs.grep: { (.key ∈ @squares) && .value.defined} -> $p {
    my $ten = (@counts[$p.key] > 10) ?? ', (first ten)' !! '';
    say "\nGap {$p.key}: {comma @counts[$p.key]} found$ten:";
    put join "\n", $p.value.batch(5)».map({"($_, {$_+ $p.key})"})».join(', ');
}
```

#### Output:
```
Adjacent primes up to 10,000,000,000 with a gap value that is a perfect square:
Gap 1: 1 found:
(2, 3)

Gap 4: 27,409,998 found, (first ten):
(7, 11), (13, 17), (19, 23), (37, 41), (43, 47)
(67, 71), (79, 83), (97, 101), (103, 107), (109, 113)

Gap 16: 15,888,305 found, (first ten):
(1831, 1847), (1933, 1949), (2113, 2129), (2221, 2237), (2251, 2267)
(2593, 2609), (2803, 2819), (3121, 3137), (3373, 3389), (3391, 3407)

Gap 36: 11,593,345 found, (first ten):
(9551, 9587), (12853, 12889), (14107, 14143), (15823, 15859), (18803, 18839)
(22193, 22229), (22307, 22343), (22817, 22853), (24281, 24317), (27143, 27179)

Gap 64: 1,434,957 found, (first ten):
(89689, 89753), (107377, 107441), (288583, 288647), (367957, 368021), (381103, 381167)
(400759, 400823), (445363, 445427), (623107, 623171), (625699, 625763), (637003, 637067)

Gap 100: 268,933 found, (first ten):
(396733, 396833), (838249, 838349), (1313467, 1313567), (1648081, 1648181), (1655707, 1655807)
(2345989, 2346089), (2784373, 2784473), (3254959, 3255059), (3595489, 3595589), (4047157, 4047257)

Gap 144: 35,563 found, (first ten):
(11981443, 11981587), (18687587, 18687731), (20024339, 20024483), (20388583, 20388727), (21782503, 21782647)
(25507423, 25507567), (27010003, 27010147), (28716287, 28716431), (31515413, 31515557), (32817493, 32817637)

Gap 196: 1,254 found, (first ten):
(70396393, 70396589), (191186251, 191186447), (208744777, 208744973), (233987851, 233988047), (288568771, 288568967)
(319183093, 319183289), (336075937, 336076133), (339408151, 339408347), (345247753, 345247949), (362956201, 362956397)

Gap 256: 41 found, (first ten):
(1872851947, 1872852203), (2362150363, 2362150619), (2394261637, 2394261893), (2880755131, 2880755387), (2891509333, 2891509589)
(3353981623, 3353981879), (3512569873, 3512570129), (3727051753, 3727052009), (3847458487, 3847458743), (4008610423, 4008610679)
```
