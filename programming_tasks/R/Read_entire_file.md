[1]: https://rosettacode.org/wiki/Read_entire_file

# [Read entire file][1]

```perl
my $string = slurp 'sample.txt';
```


The default encoding is UTF-8. The `:enc` adverb can be used to specify a different one:

```perl
my $string = slurp 'sample.txt', :enc<UTF-16>;
```


`IO::Path` objects also provide `slurp` as a method:

```perl
my $string = 'sample.txt'.IO.slurp;
```