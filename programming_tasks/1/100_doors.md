[1]: https://rosettacode.org/wiki/100_doors

# [100 doors][1]

### unoptimized

```raku
my @doors = False xx 101;
 
(.=not for @doors[0, $_ ... 100]) for 1..100;
 
say "Door $_ is ", <closed open>[ @doors[$_] ] for 1..100;
```


### optimized

```raku
say "Door $_ is open" for map {$^n ** 2}, 1..10;
```


### Here's a version using the cross meta-operator instead of a map:

```raku
 say "Door $_ is open" for 1..10 X** 2;
```


This one prints both opened and closed doors:

```raku
say "Door $_ is ", <closed open>[.sqrt == .sqrt.floor] for 1..100;
```


### verbose version, but uses ordinary components

```raku
 
sub  output( @arr, $max ) {
    my $output = 1;
    for 1..^$max -> $index {
	if @arr[$index] {
	    printf "%4d", $index;
	    say '' if $output++ %%  10;
	}
    }
    say '';
}
 
sub MAIN ( Int :$doors = 100 ) {
    my $doorcount = $doors + 1;
    my @door[$doorcount] = 0 xx ($doorcount);
 
    INDEX:
    for 1...^$doorcount -> $index {
        # flip door $index & its multiples, up to last door.
        #
	for ($index, * + $index ... *)[^$doors] -> $multiple {
	    next INDEX if $multiple > $doors;
	    @door[$multiple] =  @door[$multiple] ?? 0 !! 1;
	}
    }
    output @door, $doors+1;
}
 
```

#### Output:
```
$ ./100_doors.pl6 -doors=100
   1   4   9  16  25  36  49  64  81
```