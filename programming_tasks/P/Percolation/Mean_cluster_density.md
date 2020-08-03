[1]: https://rosettacode.org/wiki/Percolation/Mean_cluster_density

# [Percolation/Mean cluster density][1]

```raku
my @perc;
my $fill = 'x';
 
enum Direction <DeadEnd Up Right Down Left>;
 
my $𝘒 = perctest(15);
.fmt("%-2s").say for @perc;
say "𝘱 = 0.5, 𝘕 = 15, 𝘒 = $𝘒\n";
 
my $trials = 5;
for 10, 30, 100, 300, 1000 -> $𝘕 {
    my $𝘒 = ( [+] perctest($𝘕) xx $trials ) / $trials;
    say "𝘱 = 0.5, trials = $trials, 𝘕 = $𝘕, 𝘒 = $𝘒";
}
 
sub infix:<deq> ( $a, $b ) { $a.defined && ($a eq $b) }
 
sub perctest ( $grid ) {
    generate $grid;
    my $block = 1;
    for ^$grid X ^$grid -> ($y, $x) {
        fill( [$x, $y], $block++ ) if @perc[$y; $x] eq $fill
    }
    ($block - 1) / $grid²;
}
 
sub generate ( $grid ) {
    @perc = ();
    @perc.push: [ ( rand < .5 ?? '.' !! $fill ) xx $grid ] for ^$grid;
}
 
sub fill ( @cur, $block ) {
    @perc[@cur[1]; @cur[0]] = $block;
    my @stack;
    my $current = @cur;
 
    loop {
        if my $dir = direction( $current ) {
            @stack.push: $current;
            $current = move $dir, $current, $block
        }
        else {
            return unless @stack;
            $current = @stack.pop
        }
    }
 
    sub direction( [$x, $y] ) {
        ( Down  if @perc[$y + 1][$x] deq $fill ) ||
        ( Left  if @perc[$y][$x - 1] deq $fill ) ||
        ( Right if @perc[$y][$x + 1] deq $fill ) ||
        ( Up    if @perc[$y - 1][$x] deq $fill ) ||
        DeadEnd
    }
 
    sub move ( $dir, @cur, $block ) {
        my ( $x, $y ) = @cur;
        given $dir {
            when Up    { @perc[--$y; $x] = $block }
            when Down  { @perc[++$y; $x] = $block }
            when Left  { @perc[$y; --$x] = $block }
            when Right { @perc[$y; ++$x] = $block }
        }
        [$x, $y]
    }
}
 
```

#### Output:
```
.  .  1  .  2  .  .  3  .  .  .  4  .  .  . 
2  2  .  .  2  2  2  .  5  5  .  4  4  4  4 
2  2  2  2  2  .  2  .  5  .  .  4  .  .  4 
2  .  2  .  2  2  .  .  .  .  4  4  4  .  4 
.  .  .  .  .  2  .  .  .  .  4  4  4  .  . 
6  6  6  6  .  .  7  7  .  .  4  .  4  4  . 
6  .  6  .  .  .  .  .  4  4  4  4  4  .  . 
6  6  6  .  .  .  8  8  .  4  4  4  .  .  4 
6  .  .  .  9  .  .  .  .  .  .  4  4  .  4 
.  10 .  11 .  .  12 12 .  4  .  .  4  4  4 
11 .  11 11 11 11 .  12 .  4  .  4  4  4  . 
11 11 11 11 11 11 11 .  .  4  4  .  .  4  . 
11 11 11 11 .  11 .  .  .  4  4  4  4  4  . 
11 11 11 .  11 11 11 .  .  4  4  4  4  .  13
.  11 11 .  11 11 .  .  .  .  4  4  .  14 . 
𝘱 = 0.5, 𝘕 = 15, 𝘒 = 0.062222

𝘱 = 0.5, trials = 5, 𝘕 = 10, 𝘒 = 0.114
𝘱 = 0.5, trials = 5, 𝘕 = 30, 𝘒 = 0.082444
𝘱 = 0.5, trials = 5, 𝘕 = 100, 𝘒 = 0.06862
𝘱 = 0.5, trials = 5, 𝘕 = 300, 𝘒 = 0.066889
𝘱 = 0.5, trials = 5, 𝘕 = 1000, 𝘒 = 0.0659358
```