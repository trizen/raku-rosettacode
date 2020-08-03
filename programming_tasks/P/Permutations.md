[1]: https://rosettacode.org/wiki/Permutations

# [Permutations][1]

First, you can just use the built-in method on any list type.

```perl
.say for <a b c>.permutations
```

#### Output:
```
a b c
a c b
b a c
b c a
c a b
c b a
```


Here is some generic code that works with any ordered type. To force lexicographic ordering, change `after` to `gt`. To force numeric order, replace it with `>`.

```perl
sub next_perm ( @a is copy ) {
    my $j = @a.end - 1;
    return Nil if --$j < 0 while @a[$j] after @a[$j+1];
 
    my $aj = @a[$j];
    my $k  = @a.end;
    $k-- while $aj after @a[$k];
    @a[ $j, $k ] .= reverse;
 
    my $r = @a.end;
    my $s = $j + 1;
    @a[ $r--, $s++ ] .= reverse while $r > $s;
    return $(@a);
}
 
.say for [<a b c>], &next_perm ...^ !*;
```

#### Output:
```
a b c
a c b
b a c
b c a
c a b
c b a
```


Here is another non-recursive implementation, which returns a lazy list. It also works with any type.

```perl
sub permute(+@items) {
   my @seq := 1..+@items;
   gather for (^[*] @seq) -> $n is copy {
      my @order;
      for @seq {
         unshift @order, $n mod $_;
         $n div= $_;
      }
      my @i-copy = @items;
      take map { |@i-copy.splice($_, 1) }, @order;
   }
}
.say for permute( 'a'..'c' )
```

#### Output:
```
(a b c)
(a c b)
(b a c)
(b c a)
(c a b)
(c b a)
```


Finally, if you just want zero-based numbers, you can call the built-in function:

```perl
.say for permutations(3);
```

#### Output:
```
0 1 2
0 2 1
1 0 2
1 2 0
2 0 1
2 1 0
```