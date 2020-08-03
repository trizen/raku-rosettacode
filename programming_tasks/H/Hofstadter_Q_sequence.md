[1]: https://rosettacode.org/wiki/Hofstadter_Q_sequence

# [Hofstadter Q sequence][1]

### OO solution



Similar concept as the perl5 solution, except that the cache is only filled on demand.

```raku
class Hofstadter {
  has @!c = 1,1;
  method AT-POS ($me: Int $i) {
    @!c.push($me[@!c.elems-$me[@!c.elems-1]] +
	     $me[@!c.elems-$me[@!c.elems-2]]) until @!c[$i]:exists;
    return @!c[$i];
  }
}
 
# Testing:
 
my Hofstadter $Q .= new();
 
say "first ten: $Q[^10]";
say "1000th: $Q[999]";
 
my $count = 0;
$count++ if $Q[$_ +1 ] < $Q[$_] for  ^99_999;
say "In the first 100_000 terms, $count terms are less than their preceding terms";
```

#### Output:
```
first ten: 1 1 2 3 3 4 5 5 6 6
1000th: 502
In the first 100_000 terms, 49798 terms are less than their preceding terms
```


### Idiomatic solution



With a lazily generated array, we automatically get caching.

```raku
my @Q = 1, 1, -> $a, $b {
    (state $n = 1)++;
    @Q[$n - $a] + @Q[$n - $b]
} ... *;
 
# Testing:
 
say "first ten: ", @Q[^10];
say "1000th: ", @Q[999];
say "In the first 100_000 terms, ",
   [+](@Q[1..100000] Z< @Q[0..99999]),
   " terms are less than their preceding terms";
```


(Same output.)