[1]: https://rosettacode.org/wiki/Spiral_matrix

# [Spiral matrix][1]

### Object-oriented Solution



Suppose we set up a Turtle class like this:

```raku
class Turtle {
    my @dv =  [0,-1], [1,-1], [1,0], [1,1], [0,1], [-1,1], [-1,0], [-1,-1];
    my $points = 8; # 'compass' points of neighbors on grid: north=0, northeast=1, east=2, etc.
 
    has @.loc = 0,0;
    has $.dir = 0;
    has %.world;
    has $.maxegg;
    has $.range-x;
    has $.range-y;
 
    method turn-left ($angle = 90) { $!dir -= $angle / 45; $!dir %= $points; }
    method turn-right($angle = 90) { $!dir += $angle / 45; $!dir %= $points; }
 
    method lay-egg($egg) {
    %!world{~@!loc} = $egg;
    $!maxegg max= $egg;
    $!range-x minmax= @!loc[0];
    $!range-y minmax= @!loc[1];
    }
 
    method look($ahead = 1) {
    my $there = @!loc »+« @dv[$!dir] »*» $ahead;
    %!world{~$there};
    }
 
    method forward($ahead = 1) {
    my $there = @!loc »+« @dv[$!dir] »*» $ahead;
    @!loc = @($there);
    }
 
    method showmap() {
    my $form = "%{$!maxegg.chars}s";
    my $endx = $!range-x.max;
        for $!range-y.list X $!range-x.list -> ($y, $x) {
        print (%!world{"$x $y"} // '').fmt($form);
        print $x == $endx ?? "\n" !! ' ';
    }
    }
}
 
# Now we can build the spiral in the normal way from outside-in like this:
 
sub MAIN(Int $size = 5) {
my $t = Turtle.new(dir => 2);
my $counter = 0;
$t.forward(-1);
for 0..^ $size -> $ {
    $t.forward;
    $t.lay-egg($counter++);
}
for $size-1 ... 1 -> $run {
    $t.turn-right;
    $t.forward, $t.lay-egg($counter++) for 0..^$run;
    $t.turn-right;
    $t.forward, $t.lay-egg($counter++) for 0..^$run;
}
$t.showmap;
}
```


Or we can build the spiral from inside-out like this:

```raku
sub MAIN(Int $size = 5) {
my $t = Turtle.new(dir => ($size %% 2 ?? 4 !! 0));
my $counter = $size * $size;
while $counter {
    $t.lay-egg(--$counter);
    $t.turn-left;
    $t.turn-right if $t.look;
    $t.forward;
}
$t.showmap;
}
```


Note that with these "turtle graphics" we don't actually have to care about the coordinate system, since the `showmap` method can show whatever rectangle was modified by the turtle. So unlike the standard inside-out algorithm, we don't have to find the center of the matrix first.



### Procedural Solution

```raku
sub spiral_matrix ( $n ) {
    my @sm;
    my $len = $n;
    my $pos = 0;
 
    for ^($n/2).ceiling -> $i {
        my $j = $i +  1;
        my $e = $n - $j;
 
        @sm[$i     ][$i + $_] = $pos++ for         ^(  $len); # Top
        @sm[$j + $_][$e     ] = $pos++ for         ^(--$len); # Right
        @sm[$e     ][$i + $_] = $pos++ for reverse ^(  $len); # Bottom
        @sm[$j + $_][$i     ] = $pos++ for reverse ^(--$len); # Left
    }
 
    return @sm;
}
 
say .fmt('%3d') for spiral_matrix(5);
```

#### Output:
```
 0   1   2   3   4
15  16  17  18   5
14  23  24  19   6
13  22  21  20   7
12  11  10   9   8
```