[1]: http://rosettacode.org/wiki/Remove_lines_from_a_file

# [Remove lines from a file][1]

```perl
sub MAIN ($filename, $beg, $len) {
    my @lines = split /^^/, slurp $filename;
    unlink $filename;  # or rename
    splice(@lines,$beg,$len) == $len or warn "Too few lines";
    spurt $filename, @lines;
}
```

#### Output:
```
$ cal >foo
$ ./rmlines
 Usage:
   rmlines <filename> <beg> <len>
$ ./rmlines foo 1 2
$ cat foo
  1  2  3  4  5  6  7  
  8  9 10 11 12 13 14  
 15 16 17 18 19 20 21  
 22 23 24 25 26 27 28  
 29 30 31
```