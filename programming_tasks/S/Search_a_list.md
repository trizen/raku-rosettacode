[1]: https://rosettacode.org/wiki/Search_a_list

# [Search a list][1]

```raku
my @haystack = <Zig Zag Wally Ronald Bush Krusty Charlie Bush Bozo>;
 
for <Washington Bush> -> $needle {
    say "$needle -- { @haystack.first($needle, :k) // 'not in haystack' }";
}
```

#### Output:
```
Washington -- not in haystack
Bush -- 4
```




Or, including the "extra credit" task:

```raku
my Str @haystack = <Zig Zag Wally Ronald Bush Krusty Charlie Bush Bozo>;
 
for <Washingston Bush> -> $needle {
    my $first = @haystack.first($needle, :k);
 
    if defined $first {
        my $last = @haystack.first($needle, :k, :end);
        say "$needle -- first at $first, last at $last";
    }
    else {
        say "$needle -- not in haystack";
    }
}
```

#### Output:
```
Washingston -- not in haystack
Bush -- first at 4, last at 7
```


The built-in method `.first` takes a [smart-matcher](https://docs.perl6.org/language/operators#infix_~~), and returns the first matching list element.

The `:k` adverb tells it to return the key (a.k.a. list index) instead of the value of the matching element.

The `:end` adverb tells it to start searching from the end of the list.






If you plan to do many searches on the same large list, you might want to build a search hash first for efficient look-up:

```raku
my @haystack = <Zig Zag Wally Ronald Bush Krusty Charlie Bush Bozo>;
 
my %index;
%index{.value} //= .key for @haystack.pairs;
 
for <Washington Bush> -> $needle {
    say "$needle -- { %index{$needle} // 'not in haystack' }";
}
```