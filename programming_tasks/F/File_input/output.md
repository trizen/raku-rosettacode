[1]: https://rosettacode.org/wiki/File_input/output

# [File input/output][1]

If it is okay to have a temporary copy of the entire file in memory:

```perl
spurt "output.txt", slurp "input.txt";
```


Otherwise, copying line-by line:

```perl
my $in = open "input.txt";
my $out = open "output.txt", :w;
for $in.lines -> $line {
    $out.say: $line;
}
$in.close;
$out.close;
```