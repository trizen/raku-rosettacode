[1]: https://rosettacode.org/wiki/Keyboard_input/Flush_the_keyboard_buffer

# [Keyboard input/Flush the keyboard buffer][1]

Using termios to set some input attributes, flush the buffer &amp; do unbuffered reads. Longer than strictly necessary to demonstrate concepts and make it easy to verify that it actually works as advertised.

```perl
use Term::termios;
 
constant $saved   = Term::termios.new( :fd($*IN.native-descriptor) ).getattr;
constant $termios = Term::termios.new( :fd($*IN.native-descriptor) ).getattr;
 
# set some modified input flags
$termios.unset_iflags(<BRKINT ICRNL ISTRIP IXON>);
$termios.unset_lflags(< ECHO ICANON IEXTEN>);
$termios.setattr(:DRAIN);
 
# reset terminal to original settings on exit
END { $saved.setattr(:NOW) }
 
 
# Sleep for a few seconds to give you time to fill the input buffer,
# type a bunch of random characters.
sleep 2;
 
# ------- The actual task --------
# Flush the input buffer
 
$termios.setattr(:FLUSH);
 
# --------------------------------
 
# Ctrl-C to exit
loop {
    # Read up to 5 bytes from STDIN
    # F5 through F12 are 5 bytes each
    my $keypress = $*IN.read: 5;
    # print the ordinals of the keypress character
    print $keypress.decode.ords;
    print "|";
}
```