[1]: https://rosettacode.org/wiki/Take_notes_on_the_command_line

# [Take notes on the command line][1]

```perl
my $file = 'notes.txt';
 
multi MAIN() {
    print slurp($file);
}
 
multi MAIN(*@note) {
    my $fh = open($file, :a);
    $fh.say: DateTime.now, "\n\t", @note;
    $fh.close;
}
```