[1]: https://rosettacode.org/wiki/Abelian_sandpile_model

# [Abelian sandpile model][1]

Defaults to a stack of 1000 and showing progress. Pass in a custom stack size if desired and -hide-progress to run without displaying progress (much faster.)

```perl
sub cleanup { print "\e[0m\e[?25h\n"; exit(0) }
 
signal(SIGINT).tap: { cleanup(); exit(0) }
 
unit sub MAIN ($stack = 1000, :$hide-progress = False );
 
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
                ++@buffer[ $idx - $w ] if $row > 0;
                ++@buffer[ $idx - 1  ] if $idx - 1 > $rs;
                ++@buffer[ $idx + $w ] if $row < $h - 1;
                ++@buffer[ $idx + 1  ] if $idx + 1 < $buf;
                @buffer[ $idx ] -= 4;
                $done = False;
            }
        }
    }
    unless $hide-progress {
        print join '', @buffer.map( { @color[$_ min 4] })
    }
} until $done;
 
print join '', @buffer.map( { @color[$_ min 4] });
 
cleanup;
```


Passing in 2048 as a stack size results in: [Abelian-sandpile-model-perl6.png](https://github.com/thundergnat/rc/blob/master/img/Abelian-sandpile-model-perl6.png) (offsite .png image)
