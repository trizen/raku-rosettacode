[1]: https://rosettacode.org/wiki/Read_a_file_line_by_line

# [Read a file line by line][1]

The lines method is lazy so the following code does indeed read the file line by line, and not all at once.

```perl
for open('test.txt').lines
{
  .say
}
```


In order to be more explicit about the file being read on line at a time, one can write:

```perl
my $f = open 'test.txt';
while my $line = $f.get {
    say $line;
}
```