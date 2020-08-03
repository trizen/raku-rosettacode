[1]: https://rosettacode.org/wiki/Vibrating_rectangles

# [Vibrating rectangles][1]

Ok. The task description is essentially non-existent. In looking at the reference implementation (Ring) it seems like we are supposed to draw a series of concentric rectangles and then alter the colors step-wise. No actual vibration apparent.



Could fire up a GUI but WTH, let's try it at a command line with ANSI.



Draws a series of concentric rectangles then rotates through the color palette. Every three seconds, chooses new random palette colors and reverses rotation direction.

```raku
# box drawing characters
my %r = :tl<┌>, :h<─>, :tr<┐>, :v<│>, :bl<└>, :br<┘>;
 
my @colors = « \e[1;31m \e[1;94m \e[1;33m \e[1;35m \e[1;36m \e[1;32m \e[1;34m »;
 
# color palette
my @c = flat @colors[0] xx 12, @colors[3] xx 12, @colors[2] xx 12;
 
print "\e[?25l"; # hide the cursor
 
signal(SIGINT).tap: {
    print "\e[0H\e[0J\e[?25h"; # clean up on exit
    exit;
}
 
my $rot = 1;
 
my @vibe;
 
loop {
    rect($_, 31-$_) for ^15;
    display @vibe;
    @c.=rotate($rot);
    if ++$ %% 30 {
        @c = |@colors.pick(3);
        @c = sort(flat @c xx 12);
        $rot *= -1;
    }
    sleep .1;
}
 
sub rect ($b, $e) {
    @vibe[$b;$b..$e] = @c[$b % @c]~%r<tl>, |((%r<h>) xx ($e - $b - 1)), %r<tr>~"\e[0m";
    @vibe[$e;$b..$e] = @c[$b % @c]~%r<bl>, |((%r<h>) xx ($e - $b - 1)), %r<br>~"\e[0m";
    ($b ^..^ $e).map: { @vibe[$_;$b] = @vibe[$_;$e] = @c[$b % @c]~%r<v>~"\e[0m" }
}
 
sub display (@rect) {
    print "\e[0H\e[0J\n\n";
    for @rect -> @row {
        print "\t\t\t";
        print $_ // ' ' for @row;
        print "\n";
    }
}
```


See: [Vibrating rectangles](https://github.com/thundergnat/rc/blob/master/img/vibrating-rectangles-perl6.gif) (.gif image)