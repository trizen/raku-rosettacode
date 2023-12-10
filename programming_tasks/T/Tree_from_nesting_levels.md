[1]: https://rosettacode.org/wiki/Tree_from_nesting_levels

# [Tree from nesting levels][1]

### Iterative

```perl
sub new_level ( @stack --> Nil ) {
    my $e = [];
    push @stack.tail, $e;
    push @stack,      $e;
}
sub to_tree_iterative ( @xs --> List ) {
    my $nested = [];
    my @stack  = $nested;

    for @xs -> Int $x {
        new_level(@stack) while $x > @stack;
        pop       @stack  while $x < @stack;
        push @stack.tail, $x;
    }

    return $nested;
}
my @tests = (), (1, 2, 4), (3, 1, 3, 1), (1, 2, 3, 1), (3, 2, 1, 3), (3, 3, 3, 1, 1, 3, 3, 3);
say .Str.fmt( '%15s => ' ), .&to_tree_iterative for @tests;
```

#### Output:
```
                => []
          1 2 4 => [1 [2 [[4]]]]
        3 1 3 1 => [[[3]] 1 [[3]] 1]
        1 2 3 1 => [1 [2 [3]] 1]
        3 2 1 3 => [[[3] 2] 1 [[3]]]
3 3 3 1 1 3 3 3 => [[[3 3 3]] 1 1 [[3 3 3]]]
```


### Recursive

```perl
sub to_tree_recursive ( @list, $index is copy, $depth ) {
    my @so_far = gather while $index <= @list.end {
        my $t = @list[$index];

        given $t <=> $depth {
            when Order::Same {
                take $t;
            }
            when Order::More {
                ( $index, my $n1 ) = to_tree_recursive( @list, $index, $depth+1 );
                take $n1;
            }
            when Order::Less {
                $index--;
                last;
            }
        }
        $index++;
    }

    my $i = ($depth > 1) ?? $index !! -1;
    return $i, @so_far;
}
my @tests = (), (1, 2, 4), (3, 1, 3, 1), (1, 2, 3, 1), (3, 2, 1, 3), (3, 3, 3, 1, 1, 3, 3, 3);
say .Str.fmt( '%15s => ' ), to_tree_recursive( $_, 0, 1 ).[1] for @tests;
```

#### Output:
```
                => []
          1 2 4 => [1 [2 [[4]]]]
        3 1 3 1 => [[[3]] 1 [[3]] 1]
        1 2 3 1 => [1 [2 [3]] 1]
        3 2 1 3 => [[[3] 2] 1 [[3]]]
3 3 3 1 1 3 3 3 => [[[3 3 3]] 1 1 [[3 3 3]]]
```


### String Eval

```perl
use MONKEY-SEE-NO-EVAL;
sub to_tree_string_eval ( @xs --> Array ) {
    my @gap = [ |@xs, 0 ]  Z-  [ 0, |@xs ];

    my @open  = @gap.map( '[' x  * );
    my @close = @gap.map( ']' x -* );

    my @wrapped = [Z~] @open, @xs, @close.skip;

    return EVAL @wrapped.join(',').subst(:g, ']]', '],]') || '[]';
}
my @tests = (), (1, 2, 4), (3, 1, 3, 1), (1, 2, 3, 1), (3, 2, 1, 3), (3, 3, 3, 1, 1, 3, 3, 3);
say .Str.fmt( '%15s => ' ), .&to_tree_string_eval for @tests;
```

#### Output:
```
                => []
          1 2 4 => [1 [2 [[4]]]]
        3 1 3 1 => [[[3]] 1 [[3]] 1]
        1 2 3 1 => [1 [2 [3]] 1]
        3 2 1 3 => [[[3] 2] 1 [[3]]]
3 3 3 1 1 3 3 3 => [[[3 3 3]] 1 1 [[3 3 3]]]
```
