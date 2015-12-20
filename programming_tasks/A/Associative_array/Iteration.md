[1]: http://rosettacode.org/wiki/Associative_array/Iteration

# [Associative array/Iteration][1]

```perl6
my %pairs = hello => 13, world => 31, '!' => 71;
 
for %pairs.kv -> $k, $v {
    say "(k,v) = ($k, $v)";
}
 
{ say "$^a => $^b" } for %pairs.kv;
 
say "key = $_" for %pairs.keys;
 
say "value = $_" for %pairs.values;
```