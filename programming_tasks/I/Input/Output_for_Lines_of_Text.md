[1]: https://rosettacode.org/wiki/Input/Output_for_Lines_of_Text

# [Input/Output for Lines of Text][1]

Short version:

```raku
say get for ^get;
```


Verbose version:

```raku
sub do-stuff ($line) {
    say $line;
}
 
my $n = +get;
for ^$n {
    my $line = get;
    do-stuff $line;
}
```