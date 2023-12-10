[1]: https://rosettacode.org/wiki/Abelian_sandpile_model

# [Abelian sandpile model][1]





### Terminal based



Defaults to a stack of 1000 and showing progress. Pass in a custom stack size if desired and -hide-progress to run without displaying progress (much faster.)

```perl
sub cleanup { print "\e[0m\e[?25h\n"; exit(0) }

signal(SIGINT).tap: { cleanup(); exit(0) }

unit sub MAIN ($stack = 1000, :$hide-progress = False );

my @color = "\e[38;2;0;0;0m█",
            "\e[38;2;255;0;0m█",
            "\e[38;2;255;255;0m█",
            "\e[38;2;0;0;255m█",
            "\e[38;2;255;255;255m█"
            ;

my ($h, $w) = qx/stty size/.words».Int;
my $buf = $w * $h;
my @buffer = 0 xx $buf;
my $done;

@buffer[$w * ($h div 2) + ($w div 2) - 1] = $stack;

print "\e[?25l\e[48;5;232m";

repeat {
    $done = True;
    loop (my int $row; $row < $h; $row = $row + 1) {
        my int $rs = $row * $w; # row start
        my int $re = $rs  + $w; # row end
        loop (my int $idx = $rs; $idx < $re; $idx = $idx + 1) {
            if @buffer[$idx] >= 4 {
                my $grains = @buffer[$idx] div 4;
                @buffer[ $idx - $w ] += $grains if $row > 0;
                @buffer[ $idx - 1  ] += $grains if $idx - 1 >= $rs;
                @buffer[ $idx + $w ] += $grains if $row < $h - 1;
                @buffer[ $idx + 1  ] += $grains if $idx + 1 < $re;
                @buffer[ $idx ] %= 4;
                $done = False;
            }
        }
    }
    unless $hide-progress {
        print "\e[1;1H", @buffer.map( { @color[$_ min 4] }).join;
    }
} until $done;

print "\e[1;1H", @buffer.map( { @color[$_ min 4] }).join;

cleanup;
```


Passing in 2048 as a stack size results in: [Abelian-sandpile-model-perl6.png](https://github.com/thundergnat/rc/blob/master/img/Abelian-sandpile-model-perl6.png) (offsite .png image)



### SDL2 Animation

```perl
use NativeCall;
use SDL2::Raw;

my ($width, $height) = 900, 900;

unit sub MAIN ($stack = 10000);

my int ($w, $h) = 160, 160;

my $buf = $w * $h;
my @buffer = 0 xx $buf;

@buffer[$w * ($h div 2) + ($w div 2) - 1] = $stack;


SDL_Init(VIDEO);

my SDL_Window $window = SDL_CreateWindow(
    "Abelian sandpile - Raku",
    SDL_WINDOWPOS_CENTERED_MASK, SDL_WINDOWPOS_CENTERED_MASK,
    $width, $height,
    RESIZABLE
);

my SDL_Renderer $renderer = SDL_CreateRenderer( $window, -1, ACCELERATED +| TARGETTEXTURE );

my $asp_texture = SDL_CreateTexture($renderer, %PIXELFORMAT<RGB332>, STREAMING, $w, $h);

my $pixdatabuf = CArray[int64].new(0, $w, $h, $w);

my @color = 0x00, 0xDE, 0x14, 0xAA, 0xFF;

sub render {
    my int $pitch;
    my int $cursor;

    # work-around to pass the pointer-pointer.
    my $pixdata = nativecast(Pointer[int64], $pixdatabuf);
    SDL_LockTexture($asp_texture, SDL_Rect, $pixdata, $pitch);

    $pixdata = nativecast(CArray[int8], Pointer.new($pixdatabuf[0]));

    loop (my int $row; $row < $h; $row = $row + 1) {
        my int $rs = $row * $w; # row start
        my int $re = $rs  + $w; # row end
        loop (my int $idx = $rs; $idx < $re; $idx = $idx + 1) {
            $pixdata[$idx] =  @buffer[$idx] < 4 ?? @color[@buffer[$idx]] !! @color[4];
            if @buffer[$idx] >= 4 {
                my $grains = floor @buffer[$idx] / 4;
                @buffer[ $idx - $w ] += $grains if $row > 0;
                @buffer[ $idx - 1  ] += $grains if $idx - 1 >= $rs;
                @buffer[ $idx + $w ] += $grains if $row < $h - 1;
                @buffer[ $idx + 1  ] += $grains if $idx + 1 < $re;
                @buffer[ $idx ] %= 4;
            }
        }
    }

    SDL_UnlockTexture($asp_texture);

    SDL_RenderCopy($renderer, $asp_texture, SDL_Rect, SDL_Rect.new(:x(0), :y(0), :w($width), :h($height)));
    SDL_RenderPresent($renderer);
}

my $event = SDL_Event.new;

main: loop {

    while SDL_PollEvent($event) {
        my $casted_event = SDL_CastEvent($event);

        given $casted_event {
            when *.type == QUIT {
                last main;
            }
        }
    }

    render();
    print fps;
}

say '';

sub fps {
    state $fps-frames = 0;
    state $fps-now    = now;
    state $fps        = '';
    $fps-frames++;
    if now - $fps-now >= 1 {
        $fps = [~] "\b" x 40, ' ' x 20, "\b" x 20 ,
            sprintf "FPS: %5.2f  ", ($fps-frames / (now - $fps-now)).round(.01);
        $fps-frames = 0;
        $fps-now = now;
    }
    $fps
}
```


Passing in a stack size of 20000 results in: [Abelian-sandpile-sdl2.png](https://github.com/thundergnat/rc/blob/master/img/Abelian-sandpile-sdl2.png) (offsite .png image)
