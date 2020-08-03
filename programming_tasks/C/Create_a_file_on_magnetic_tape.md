[1]: https://rosettacode.org/wiki/Create_a_file_on_magnetic_tape

# [Create a file on magnetic tape][1]

```raku
my $tape = open "/dev/tape", :w or die "Can't open tape: $!";
$tape.say: "I am a tape file now, or hope to be soon.";
$tape.close;
```