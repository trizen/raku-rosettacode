[1]: https://rosettacode.org/wiki/Random_Latin_squares

# [Random Latin squares][1]



```perl
sub latin-square { [[0],] };

sub random ( @ls, :$size = 5 ) {

    # Build
    for 1 ..^ $size -> $i {
        @ls[$i] = @ls[0].clone;
        @ls[$_].splice($_, 0, $i) for 0 .. $i;
    }

    # Shuffle
    @ls = @ls[^$size .pick(*)];
    my @cols = ^$size .pick(*);
    @ls[$_] = @ls[$_][@cols] for ^@ls;

    # Some random Latin glyphs
    my @symbols = ('A' .. 'Z').pick($size);

    @ls.deepmap: { $_ = @symbols[$_] };

}

sub display ( @array ) { $_.fmt("%2s ").put for |@array, '' }


# The Task

# Default size 5
display random latin-square;

# Specified size
display random :size($_), latin-square for 5, 3, 9;

# Or, if you'd prefer:
display random latin-square, :size($_) for 12, 2, 1;
```

#### Output:
```
 V   Z   M   J   U 
 Z   M   U   V   J 
 U   J   V   M   Z 
 J   V   Z   U   M 
 M   U   J   Z   V 
   
 B   H   K   U   D 
 H   D   U   B   K 
 K   U   H   D   B 
 U   B   D   K   H 
 D   K   B   H   U 
   
 I   P   Y 
 P   Y   I 
 Y   I   P 
   
 Y   J   K   E   Z   B   I   W   H 
 E   Y   B   W   K   H   J   Z   I 
 B   K   Y   H   J   E   Z   I   W 
 I   H   W   J   E   Z   B   Y   K 
 J   I   Z   Y   W   K   H   E   B 
 W   E   H   Z   B   I   Y   K   J 
 H   B   E   I   Y   W   K   J   Z 
 K   Z   J   B   I   Y   W   H   E 
 Z   W   I   K   H   J   E   B   Y 
   
 L   Q   E   M   A   T   Z   C   N   Y   R   D 
 Q   R   Y   L   N   D   C   E   M   T   A   Z 
 E   Y   M   C   D   Q   A   N   Z   L   T   R 
 M   L   C   N   R   Y   D   Z   A   E   Q   T 
 N   M   Z   A   Q   E   T   D   R   C   L   Y 
 T   D   Q   Y   C   A   M   L   E   R   Z   N 
 R   A   T   Q   M   Z   E   Y   L   D   N   C 
 D   Z   R   T   E   N   L   Q   Y   A   C   M 
 Y   T   L   E   Z   R   N   M   C   Q   D   A 
 A   N   D   R   L   C   Y   T   Q   Z   M   E 
 Z   C   A   D   Y   M   Q   R   T   N   E   L 
 C   E   N   Z   T   L   R   A   D   M   Y   Q 
   
 Y   G 
 G   Y 
   
 I
```
