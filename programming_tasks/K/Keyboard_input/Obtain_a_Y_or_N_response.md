[1]: http://rosettacode.org/wiki/Keyboard_input/Obtain_a_Y_or_N_response

# [Keyboard input/Obtain a Y or N response][1]

```perl
my $TTY = open("/dev/tty");
 
sub prompt-char($prompt) {
    ENTER shell "stty raw -echo min 1 time 1";
    LEAVE shell "stty sane";
 
    print $prompt;
    $TTY.getc;
}
 
say so prompt-char("Y or N? ") ~~ /:i y/;
```