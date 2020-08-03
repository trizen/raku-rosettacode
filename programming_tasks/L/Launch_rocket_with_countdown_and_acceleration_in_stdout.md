[1]: https://rosettacode.org/wiki/Launch_rocket_with_countdown_and_acceleration_in_stdout

# [Launch rocket with countdown and acceleration in stdout][1]

Uses ANSI graphics. Works best in a 24 bit ANSI terminal at least 80x24, though bigger is better.



This is a very simple simulation. It assumes a constant ~2G+ acceleration in a gravitational field; so net +10 meters per second². It completely neglects the effects of [air friction](https://en.wikipedia.org/wiki/Drag_(physics)), [impulse](https://en.wikipedia.org/wiki/Impulse), [snap, crackle &amp; pop](https://en.wikipedia.org/wiki/Snap,_Crackle_and_Pop#Physics) and has an unrealistically clean fuel burn (no contrail). It does however (unlike most of the entries at this time) start at the base of the terminal (on the ground) and go up, rather than starting at the top and dropping a contrail. It calculates and displays an accurate displacement and velocity over time and uses those to scale its vertical screen displacement.



The motion is a little "notchy" as the vertical resolution in a terminal is rather low. Exits after the rocket leaves the visible area of the terminal. See the [example animated gif](https://github.com/thundergnat/rc/blob/master/img/rocket-perl6.gif)

```raku
signal(SIGINT).tap: { cleanup() }
 
my ($rows,$cols) = qx/stty size/.words;
my $v = floor $rows - 9;
my $h = floor $cols / 2 - 4;
my $a = 0;
my $start = now;
my $t = -5;
my $i = 0;
my $j = 0;
 
my @r = Q'   |',   Q'  /_\',  Q'  | |',  Q' /| |\', Q'/_|_|_\';
my @x = Q' (/|\)', Q' {/|\}', Q'  \|/',  Q'   |';
my @y = Q'  /|\',  Q' // \\', Q' (/ \)', Q'  \ /'; #'
 
my $sp = ' ' x $h;
 
my $altitude = 0;
my $velocity = 0;
 
my @pal = "\e[38;2;255;0;0m", "\e[38;2;255;255;0m", "\e[38;2;255;155;0m";
constant \W = "\e[38;2;255;255;255m";
 
print "\e[?25l\e[48;5;232m";
 
loop {
    if $t >= 0 {
        $velocity = 5 * $t²;
        $altitude = $velocity * $t / 2;
        $a = ($altitude / $v).round;
    }
    cleanup() if $a > $rows + 5;
 
    print "\e[H\e[J", "\n" xx $v, W, $sp, @r.join("\n$sp"), "\n", $sp;
    if $t < 0 {
        print Q'\/   \/'
    } else {
        exhaust( @pal[$i], $a )
    }
    print W, "\n", '▔' x $cols, @pal[1];
    printf "\n Time: T %-4s %9s  Altitude: %6.2f meters  Velocity: %5.1f m/sec\n",
    $t < 0 ?? "- {$t.round.abs}" !! "+ {$t.round}",
    $t < 0 ?? '' !! $a == 0 ?? 'Ignition!' !! 'Lift-off!',
    $altitude.round(.01), $velocity.round(.1);
 
    ++$i;
    $i %= 3;
    ++$j;
    $j %= 2;
    $t = (now - $start - 5);
    sleep .05;
}
 
sub exhaust ($clr, $a) {
    print Q'\/', $clr, Q'/^\', W, Q'\/'; #'
    return if $a == 0;
    if $a < 4 {
        print "\n", $clr,
        $sp, ( $j ?? @x[^$a].join("\n$sp") !! @y[^$a].join("\n$sp"))
    } else {
        print "\n", $clr,
        $sp, ( $j ?? @x.join("\n$sp") !! @y.join("\n$sp"));
        print "\n" x $a - 4;
    }
}
 
# clean up on exit, reset ANSI codes, scroll, re-show the cursor & clear screen
sub cleanup () {  print "\e[0m", "\n" xx 50, "\e[H\e[J\e[?25h"; exit(0) }
```


See [rocket-perl6.gif](https://github.com/thundergnat/rc/blob/master/img/rocket-perl6.gif) (offsite animated gif image)
