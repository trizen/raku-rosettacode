[1]: https://rosettacode.org/wiki/File_size

# [File size][1]



```perl
say 'input.txt'.IO.s;
say '/input.txt'.IO.s;
```


Cross-platform version of the second one:

```perl
say $*SPEC.rootdir.IO.child("input.txt").s;
```
