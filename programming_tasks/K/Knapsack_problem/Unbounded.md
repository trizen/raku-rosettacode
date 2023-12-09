[1]: https://rosettacode.org/wiki/Knapsack_problem/Unbounded

# [Knapsack problem/Unbounded][1]

Brute force, looked a lot at the Ruby solution.

```perl
class KnapsackItem {
  has $.volume;
  has $.weight;
  has $.value;
  has $.name;
 
  method new($volume,$weight,$value,$name) {
    self.bless(:$volume, :$weight, :$value, :$name)
  }
};
 
my KnapsackItem $panacea .= new: 0.025, 0.3, 3000, "panacea";
my KnapsackItem $ichor   .= new: 0.015, 0.2, 1800, "ichor";
my KnapsackItem $gold    .= new: 0.002, 2.0, 2500, "gold";
my KnapsackItem $maximum .= new: 0.25,  25,  0   , "max";
 
my $max_val = 0;
my @solutions;
my %max_items;
 
for $panacea, $ichor, $gold -> $item {
    %max_items{$item.name} = floor [min]
                            $maximum.volume / $item.volume,
			    $maximum.weight / $item.weight;
}
 
for 0..%max_items<panacea>
       X 0..%max_items<ichor>
           X 0..%max_items<gold>
 -> ($p, $i, $g)
{
  next if $panacea.volume * $p + $ichor.volume * $i + $gold.volume * $g > $maximum.volume;
  next if $panacea.weight * $p + $ichor.weight * $i + $gold.weight * $g > $maximum.weight;
  given $panacea.value * $p + $ichor.value * $i + $gold.value * $g {
    if $_ > $max_val { $max_val = $_; @solutions = (); }
    when $max_val    { @solutions.push: $[$p,$i,$g] }
  }
}
 
say "maximum value is $max_val\npossible solutions:";
say "panacea\tichor\tgold";
.join("\t").say for @solutions;
```


#### Output:


#### Output:
```
maximum value is 54500
possible solutions:
panacea ichor   gold
0       15      11
3       10      11
6       5       11
9       0       11
```