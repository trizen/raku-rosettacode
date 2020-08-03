[1]: https://rosettacode.org/wiki/Self-describing_numbers

# [Self-describing numbers][1]

```raku
my @values = <1210 2020 21200 3211000
42101000 521001000 6210001000 27 115508>;
 
for @values -> $test {
    say "$test is {sdn($test) ?? '' !! 'NOT ' }a self describing number.";
}
 
sub sdn($n) {
    my $s = $n.Str;
    my $chars = $s.chars;
    my @a = +«$s.comb;
    my @b;
    for @a -> $i {
        return False if $i >= $chars;
        ++@b[$i];
    }
    @b[$_] //= 0 for ^$chars;
    @a eqv @b;
}
 
.say if .&sdn for ^9999999;
```


Output:


#### Output:
```
1210 is a self describing number.
2020 is a self describing number.
21200 is a self describing number.
3211000 is a self describing number.
42101000 is a self describing number.
521001000 is a self describing number.
6210001000 is a self describing number.
27 is NOT a self describing number.
115508 is NOT a self describing number.
1210
2020
21200
3211000
```