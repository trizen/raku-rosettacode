[1]: https://rosettacode.org/wiki/Read_a_specific_line_from_a_file

# [Read a specific line from a file][1]

```raku
say lines[6] // die "Short file";
```


Without an argument, the `lines` function reads filenames from the command line, or defaults to standard input. It then returns a lazy list, which we subscript to get the 7th element. Assuming this code is in a program called `line7`:


#### Output:
```
$ cal 2011 > cal.txt
$ line7 cal.txt
16 17 18 19 20 21 22  20 21 22 23 24 25 26  20 21 22 23 24 25 26  
$
```


This works even on infinite files because lists are lazy:


#### Output:
```
$ yes | line7
y
$
```