[1]: https://rosettacode.org/wiki/Fixed_length_records

# [Fixed length records][1]

Link to a copy of the input file used: [flr-infile.dat](https://github.com/thundergnat/rc/blob/master/resouces/flr-infile.dat)



Essentially the same as task [Selective_File_Copy](https://rosettacode.org/wiki/Selective_File_Copy) except more boring.

```raku
$*OUT = './flr-outfile.dat'.IO.open(:w, :bin) orelse .die; # open a file in binary mode for writing
while my $record = $*IN.read(80) {                       # read in fixed sized binary chunks
     $*OUT.write: $record.=reverse;                      # write reversed records out to $outfile
     $*ERR.say: $record.decode('ASCII');                 # display decoded records on STDERR
}
close $*OUT;
```

#### Output:
```
8.........7.........6.........5.........4.........3.........2.........1...1 eniL
                                                                          2 eniL
                                                                          3 eniL
                                                                          4 eniL
                                                                                
                                                                          6 eniL
                                                                          7 eniL
............................................................8 enil detnednI     
NIGRAM TR                                                                 9 eniL
```