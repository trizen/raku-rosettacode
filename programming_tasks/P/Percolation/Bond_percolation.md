[1]: https://rosettacode.org/wiki/Percolation/Bond_percolation

# [Percolation/Bond percolation][1]

Starts "filling" from the top left. Fluid flow favours directions in Down, Left, Right, Up order. I interpreted p to be porosity, so small p mean low permeability, large p means high permeability.

```perl
my @bond;
my $grid = 10;
my $geom = $grid - 1;
my $water = '▒';
 
enum Direction <DeadEnd Up Right Down Left>;
 
say 'Sample percolation at .6';
percolate .6;
.join.say for @bond;
say "\n";
 
my $tests = 100;
say "Doing $tests trials at each porosity:";
for .1, .2 ... 1 -> $p {
    printf "p = %0.1f: %0.2f\n", $p, (sum percolate($p) xx $tests) / $tests
}
 
sub percolate ( $prob ) {
    generate $prob;
    my @stack;
    my $current = [1;0];
    $current.&fill;
 
    loop {
        if my $dir = direction( $current ) {
            @stack.push: $current;
            $current = move $dir, $current
        }
        else {
            return 0 unless @stack;
            $current = @stack.pop
        }
        return 1 if $current[1] == +@bond - 1
    }
 
    sub direction( [$x, $y] ) {
        ( Down  if @bond[$y + 1][$x].contains: ' ' ) ||
        ( Left  if @bond[$y][$x - 1].contains: ' ' ) ||
        ( Right if @bond[$y][$x + 1].contains: ' ' ) ||
        ( Up    if @bond[$y - 1][$x].defined && @bond[$y - 1][$x].contains: ' ' ) ||
        DeadEnd
    }
 
    sub move ( $dir, @cur ) {
        my ( $x, $y ) = @cur;
        given $dir {
            when Up    { [$x,--$y].&fill xx 2 }
            when Down  { [$x,++$y].&fill xx 2 }
            when Left  { [--$x,$y].&fill xx 2 }
            when Right { [++$x,$y].&fill xx 2 }
        }
        [$x, $y]
    }
 
    sub fill ( [$x, $y] ) { @bond[$y;$x].=subst(' ', $water, :g) }
}
 
sub generate ( $prob = .5 ) {
    @bond = ();
    my $sp = '   ';
    append @bond, [flat '│', ($sp, ' ') xx $geom, $sp, '│'],
                  [flat '├', (h(), '┬') xx $geom, h(), '┤'];
    append @bond, [flat '│', ($sp, v()) xx $geom, $sp, '│'],
                  [flat '├', (h(), '┼') xx $geom, h(), '┤'] for ^$geom;
    append @bond, [flat '│', ($sp, v()) xx $geom, $sp, '│'],
                  [flat '├', (h(), '┴') xx $geom, h(), '┤'],
                  [flat '│', ($sp, ' ') xx $geom, $sp, '│'];
 
    sub h () { rand < $prob ?? $sp !! '───' }
    sub v () { rand < $prob ?? ' ' !! '│'   }
}
```

#### Output:
```
Sample percolation at .6
│▒▒▒                                    │
├▒▒▒┬   ┬───┬   ┬   ┬   ┬   ┬   ┬───┬   ┤
│▒▒▒▒▒▒▒                │   │           │
├───┼▒▒▒┼   ┼   ┼   ┼   ┼   ┼───┼   ┼   ┤
│▒▒▒▒▒▒▒▒▒▒▒│   │   │   │   │   │       │
├▒▒▒┼───┼▒▒▒┼   ┼───┼   ┼───┼   ┼   ┼   ┤
│▒▒▒│▒▒▒▒▒▒▒▒▒▒▒                │   │   │
├▒▒▒┼───┼───┼▒▒▒┼   ┼   ┼───┼   ┼   ┼   ┤
│▒▒▒│        ▒▒▒│   │   │           │   │
├───┼   ┼   ┼▒▒▒┼───┼   ┼   ┼   ┼   ┼───┤
│           │▒▒▒    │                   │
├   ┼───┼   ┼▒▒▒┼───┼───┼───┼───┼   ┼───┤
│           │▒▒▒│                       │
├───┼   ┼───┼▒▒▒┼───┼───┼───┼───┼   ┼───┤
│▒▒▒▒▒▒▒▒▒▒▒▒▒▒▒        │       │       │
├▒▒▒┼▒▒▒┼───┼▒▒▒┼───┼   ┼───┼   ┼   ┼   ┤
│▒▒▒│▒▒▒▒▒▒▒│▒▒▒▒▒▒▒│                   │
├▒▒▒┼───┼───┼───┼───┼───┼   ┼   ┼   ┼   ┤
│▒▒▒▒▒▒▒    │       │   │               │
├▒▒▒┼▒▒▒┼───┼───┼   ┼───┼───┼   ┼   ┼   ┤
│▒▒▒│▒▒▒    │               │           │
├───┴▒▒▒┴   ┴   ┴   ┴───┴   ┴   ┴   ┴───┤
│    ▒▒▒                                │


Doing 100 trials at each porosity:
p = 0.1: 0.00
p = 0.2: 0.00
p = 0.3: 0.00
p = 0.4: 0.05
p = 0.5: 0.42
p = 0.6: 0.92
p = 0.7: 1.00
p = 0.8: 1.00
p = 0.9: 1.00
p = 1.0: 1.00
```