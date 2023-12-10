[1]: https://rosettacode.org/wiki/Reverse_the_order_of_lines_in_a_text_file_while_preserving_the_contents_of_each_line

# [Reverse the order of lines in a text file while preserving the contents of each line][1]

Not going to bother testing with the task recommended file. It demonstrates nothing to do with file handling, record separators, memory conservation or anything useful. May as well just be "Reverse this list" for all the good it does.



### Lots of assumptions



Simplest thing that could possibly satisfy the extremely vague task description and completely glossing over all of the questions raised on the discussion page.



ASSUMPTIONS:

```perl
.put for reverse lines
```


### Few assumptions



Processes a small (configurable) number of bytes at a time so file can be multi-terabyte and it will handle with ease. *Does* assume Latin 1 for reduced complexity.



No assumptions were made concerning line/record termination, full stop.



Run the following to generate nul.txt. (digits 1 through 6 repeated 8 times with double null as record separators):


```
   raku -e'print join "\x00\x00", (1..6).map: * x 8' > nul.txt
```
```perl
my $input-record-separator = "\x00\x00";

my $fh = open("nul.txt".IO, :r, :bin);
$fh.seek(0, SeekFromEnd); # start at the end of the file

my $bytes = 5 min $fh.tell - 1; # read in file 5 bytes at a time (or whatever)

$fh.seek(-$bytes, SeekFromCurrent);

my $buffer = $fh.read($bytes).decode('Latin1'); # assume Latin1 for reduced complexity

loop {
    my $seek = ($fh.tell < $bytes * 2) ?? -$fh.tell !! -$bytes * 2;
    $fh.seek($seek, SeekFromCurrent);
    $buffer = $buffer R~ $fh.read((-$seek - $bytes) max 0).decode('Latin1');
    if $buffer.contains: $input-record-separator {
        my @rest;
        ($buffer, @rest) = $buffer.split: $input-record-separator;
        .say for reverse @rest; # emit any full records that have been processed
    }
    last if $fh.tell < $bytes;
}

say $buffer; # emit any remaining record
```

#### Output:
```
66666666
55555555
44444444
33333333
22222222
11111111
```
