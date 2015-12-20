[1]: http://rosettacode.org/wiki/Search_a_list

# [Search a list][1]

```perl6
sub find ($matcher, $container) {
    for $container.kv -> $k, $v {
        $v ~~ $matcher and return $k;
    }
    fail 'No values matched';
}
 
my Str @haystack = <Zig Zag Wally Ronald Bush Krusty Charlie Bush Bozo>;
 
for <Washingston Bush> -> $needle {
    my $pos = find $needle, @haystack;
    if defined $pos {
        say "Found '$needle' at index $pos";
        say 'Largest index: ', @haystack.end -
            find { $needle eq $^x }, reverse @haystack;
    }
    else {
        say "'$needle' not in haystack";
    }
}
```


The `~~` operator does smart matching based on the type of the matcher; in the general case, you can pass any predicate you like to force the semantics one way or another.