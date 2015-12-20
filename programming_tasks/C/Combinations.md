[1]: http://rosettacode.org/wiki/Combinations

# [Combinations][1]

There actually is a builtin:

```perl6
.say for combinations(5,3);
```

#### Output:
```
0 1 2
0 1 3
0 1 4
0 2 3
0 2 4
0 3 4
1 2 3
1 2 4
1 3 4
2 3 4
```


Here is an iterative routine with the same output:

```perl6
sub combinations(Int $n, Int $k) {
    return ([],) unless $k;
    return if $k > $n || $n <= 0;
    my @c = ^$k;
    gather loop {
      take [@c];
      next if @c[$k-1]++ < $n-1;
      my $i = $k-2;
      $i-- while $i >= 0 && @c[$i] >= $n-($k-$i);
      last if $i < 0;
      @c[$i]++;
      while ++$i < $k { @c[$i] = @c[$i-1] + 1; }
    }
}
.say for combinations(5,3);
```


or recursive with same output, but much slower:

```perl6
sub combinations(Int $n, Int $k) {
    return [] if $k == 0;
    return () if $k > $n;
    gather {
        take [0, (1..^$n)[@$_]] for combinations($n-1, $k-1);
        take [(1..^$n)[@$_]]    for combinations($n-1, $k  );
    }
}
 
.say for combinations(5, 3);
```