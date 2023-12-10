[1]: https://rosettacode.org/wiki/Knapsack_problem/Bounded

# [Knapsack problem/Bounded][1]





#### Original



Recursive algorithm, with cache. Idiomatic code style, using multi-subs and a class.

```perl
my class KnapsackItem { has $.name; has $.weight; has $.unit; }

multi sub pokem ([],           $,  $v = 0) { $v }
multi sub pokem ([$,  *@],     0,  $v = 0) { $v }
multi sub pokem ([$i, *@rest], $w, $v = 0) {
  my $key = "{+@rest} $w $v";
  (state %cache){$key} or do {
    my @skip = pokem @rest, $w, $v;
    if $w >= $i.weight { # next one fits
      my @put = pokem @rest, $w - $i.weight, $v + $i.unit;
      return (%cache{$key} = |@put, $i.name).list if @put[0] > @skip[0];
    }
    return (%cache{$key} = |@skip).list;
  }
}

my $MAX_WEIGHT = 400;
my @table = flat map -> $name,  $weight,  $unit,     $count {
     KnapsackItem.new( :$name, :$weight, :$unit ) xx $count;
},
        'map',                         9,      150,    1,
        'compass',                     13,     35,     1,
        'water',                       153,    200,    2,
        'sandwich',                    50,     60,     2,
        'glucose',                     15,     60,     2,
        'tin',                         68,     45,     3,
        'banana',                      27,     60,     3,
        'apple',                       39,     40,     3,
        'cheese',                      23,     30,     1,
        'beer',                        52,     10,     3,
        'suntan cream',                11,     70,     1,
        'camera',                      32,     30,     1,
        'T-shirt',                     24,     15,     2,
        'trousers',                    48,     10,     2,
        'umbrella',                    73,     40,     1,
        'waterproof trousers',         42,     70,     1,
        'waterproof overclothes',      43,     75,     1,
        'note-case',                   22,     80,     1,
        'sunglasses',                  7,      20,     1,
        'towel',                       18,     12,     2,
        'socks',                       4,      50,     1,
        'book',                        30,     10,     2
        ;

my ($value, @result) = pokem @table, $MAX_WEIGHT;

(my %hash){$_}++ for @result;

say "Value = $value";
say "Tourist put in the bag:";
say "  # ITEM";
for %hash.sort -> $item {
  say "  {$item.value} {$item.key}";
}
```

#### Output:
```
Value = 1010
Tourist put in the bag:
  # ITEM
  3 banana
  1 cheese
  1 compass
  2 glucose
  1 map
  1 note-case
  1 socks
  1 sunglasses
  1 suntan cream
  1 water
  1 waterproof overclothes
```


#### Faster alternative



Also recursive, with cache, but substantially faster.  Code more generic (ported from Perl solution).

```perl
my $raw = qq:to/TABLE/;
map             9       150     1
compass         13      35      1
water           153     200     2
sandwich        50      60      2
glucose         15      60      2
tin             68      45      3
banana          27      60      3
apple           39      40      3
cheese          23      30      1
beer            52      10      1
suntancream     11      70      1
camera          32      30      1
T-shirt         24      15      2
trousers        48      10      2
umbrella        73      40      1
w_trousers      42      70      1
w_overcoat      43      75      1
note-case       22      80      1
sunglasses       7      20      1
towel           18      12      2
socks            4      50      1
book            30      10      2
TABLE

my @items;
for split(["\n", /\s+/], $raw, :skip-empty) -> $n,$w,$v,$q {
    @items.push: %{ name => $n, weight => $w, value => $v, quant => $q}
}

my $max_weight = 400;

sub pick ($weight, $pos) {
    state %cache;
    return 0, 0 if $pos < 0 or $weight <= 0;

    my $key = $weight ~ $pos;
    %cache{$key} or do {
        my %item = @items[$pos];
        my ($bv, $bi, $bw, @bp) = (0, 0, 0);

        for 0 .. %item{'quant'} -> $i {
            last if $i * %item{'weight'} > $weight;
            my ($v, $w, @p) = pick($weight - $i * %item{'weight'}, $pos - 1);
            next if ($v += $i * %item{'value'}) <= $bv;

            ($bv, $bi, $bw, @bp) = ($v, $i, $w, |@p);
        }
        %cache{$key} = $bv, $bw + $bi * %item{'weight'}, |@bp, $bi;
    }
}

my ($v, $w, @p) = pick($max_weight, @items.end);
{ say "{@p[$_]} of @items[$_]{'name'}" if @p[$_] } for 0 .. @p.end;
say "Value: $v Weight: $w";
```

#### Output:
```
1 of map
1 of compass
1 of water
2 of glucose
3 of banana
1 of cheese
1 of suntancream
1 of w_overcoat
1 of note-case
1 of sunglasses
1 of socks
Value: 1010 Weight: 396
```
