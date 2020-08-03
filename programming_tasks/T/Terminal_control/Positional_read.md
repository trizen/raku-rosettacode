[1]: https://rosettacode.org/wiki/Terminal_control/Positional_read

# [Terminal control/Positional read][1]

```perl
#!/usr/bin/env perl6

use v6;
use NCurses;

# Reference:
# https://github.com/azawawi/perl6-ncurses

# Initialize curses window
my $win = initscr() or die "Failed to initialize ncurses\n";

# Print random text in a 10x10 grid

for ^10 { mvaddstr($_ , 0, (for ^10 {(41 .. 90).roll.chr}).join )};

# Read

my $icol = 3 - 1;
my $irow = 6 - 1;

my $ch = mvinch($irow,$icol);

# Show result

mvaddstr($irow, $icol+10, 'Character at column 3, row 6 = ' ~ $ch.chr);

mvaddstr( LINES() - 2, 2, "Press any key to exit..." );

# Refresh (this is needed)
nc_refresh;

# Wait for a keypress
getch;

# Cleanup
LEAVE {
    delwin($win) if $win;
    endwin;
}
```

#### Output:
```
+W18:5I<1N
N-I.HG45SK
BFJY8:AK)8
J+4U<H1++:
RP>BX-/19Y
URDESVX;HX  Character at column 3, row 6 = D
J7+X3@E<BG
M;?2U<8+FI
)@BG,:D)O1
)>A-=LDY-.












  Press any key to exit...
```
