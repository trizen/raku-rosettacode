[1]: http://rosettacode.org/wiki/Input_loop

# [Input loop][1]

In Perl 6, filehandles etc. provide the `.lines` and `.words` methods which return lazy lists, and can thus they be iterated using a `for` loop...



*Line-by-line* <small>_(line endings are automatically stripped)_</small>

```perl6
for "filename.txt".IO.lines -> $line {
    ...
}
```
```perl6
for $*IN.lines -> $line {
    ...
}
```
```perl6
for pipe("find -iname '*.txt'").lines -> $filename {
    ...
}
```
```perl6
for pipe("find -iname '*.txt' -print0", :nl«\0»).lines -> $filename {
    ...
}
```


*Word-by-word*

```perl6
for "filename.txt".IO.words -> $word {
    ...
}
```