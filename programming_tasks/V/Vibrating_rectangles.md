[1]: https://rosettacode.org/wiki/Vibrating_rectangles

# [Vibrating rectangles][1]





### ANSI graphics



Ok. The task description is essentially non-existent. In looking at the reference implementation (Ring) it seems like we are supposed to draw a series of concentric rectangles and then alter the colors step-wise. No actual vibration apparent.



Could fire up a GUI but WTH, let's try it at a command line with ANSI.



Draws a series of concentric rectangles then rotates through the color palette. Every three seconds, chooses new random palette colors and reverses rotation direction.

```perl
# box drawing characters
my %r = :tl<┌>, :h<─>, :tr<┐>, :v<│>, :bl<└>, :br<┘>;

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
    if ++$ %% 30 {
        @c = |@colors.pick(3);
        @c = sort(flat @c xx 12);
        $rot *= -1;
    }
    sleep .1;
}

sub rect ($b, $e) {
    @vibe[$b;$b..$e] = @c[$b % @c]~%r<tl>, |((%r<h>) xx ($e - $b - 1)), %r<tr>~"\e[0m";
    @vibe[$e;$b..$e] = @c[$b % @c]~%r<bl>, |((%r<h>) xx ($e - $b - 1)), %r<br>~"\e[0m";
    ($b ^..^ $e).map: { @vibe[$_;$b] = @vibe[$_;$e] = @c[$b % @c]~%r<v>~"\e[0m" }
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



### SDL Animation



Fully animated SDL2 graphics lib version. Will adjust rendered rectangles to fill resized windows. Hit the space bar to toggle palette rotation direction.

```perl
use SDL2::Raw;

my $width  = 1200;
my $height = 800;

SDL_Init(VIDEO);

my $window = SDL_CreateWindow(
    'Vibrating rectangles',
    SDL_WINDOWPOS_CENTERED_MASK,
    SDL_WINDOWPOS_CENTERED_MASK,
    $width, $height,
    RESIZABLE
);

my $render = SDL_CreateRenderer($window, -1, ACCELERATED +| PRESENTVSYNC);

my $event = SDL_Event.new;

enum KEY_CODES (
    K_SPACE  => 44,
);

my $num = 80;
my @rgb = palette($num);
my ($cx, $cy);
my $dir = 1;

main: loop {
    while SDL_PollEvent($event) {
        my $casted_event = SDL_CastEvent($event);
        given $casted_event {
            when *.type == QUIT { last main }
            when *.type == WINDOWEVENT {
                if .event == 5 {
                    $width  = .data1;
                    $height = .data2;
                }
            }
            when *.type == KEYDOWN {
                if KEY_CODES(.scancode) -> $comm {
                    given $comm {
                        when 'K_SPACE'  { $dir *= -1; }
                    }
                }
                #say .scancode; # unknown key pressed
            }
        }
    }
    ($cx, $cy) = $width div 2, $height div 2;

    for 1..^$num {
        my ($x, $y) = ($cx - ($width/2/$num*$_), $cy - ($height/2/$num*$_))».round;
        my ($w, $h) = ($width/$num*$_, $height/$num*$_)».round;
        SDL_SetRenderDrawColor($render, |@rgb[$_], 255);
        SDL_RenderDrawRect($render, SDL_Rect.new(:x($x), :y($y), :w($w), :h($h)));
    }
    @rgb.=rotate($dir);
    SDL_RenderPresent($render);
    SDL_SetRenderDrawColor($render, 0, 0, 0, 0);
    SDL_RenderClear($render);
}

SDL_Quit();

sub palette ($l) { (^$l).map: { hsv2rgb(($_ * 360/$l % 360)/360, 1, 1).list } };

sub hsv2rgb ( $h, $s, $v ){ # inputs normalized 0-1
    my $c = $v * $s;
    my $x = $c * (1 - abs( (($h*6) % 2) - 1 ) );
    my $m = $v - $c;
    my ($r, $g, $b) = do given $h {
        when   0..^(1/6) { $c, $x, 0 }
        when 1/6..^(1/3) { $x, $c, 0 }
        when 1/3..^(1/2) { 0, $c, $x }
        when 1/2..^(2/3) { 0, $x, $c }
        when 2/3..^(5/6) { $x, 0, $c }
        when 5/6..1      { $c, 0, $x }
    }
    ( $r, $g, $b ).map: ((*+$m) * 255).Int
}
```
