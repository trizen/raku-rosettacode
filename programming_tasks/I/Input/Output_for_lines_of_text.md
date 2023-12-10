[1]: https://rosettacode.org/wiki/Input/Output_for_lines_of_text

# [Input/Output for lines of text][1]





Short version:

```perl
say get for ^get;
```


Verbose version:

```perl
sub do-stuff ($line) {
    say $line;
}
 
my $n = +get;
for ^$n {
    my $line = get;
    do-stuff $line;
}
```
