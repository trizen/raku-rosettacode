[1]: https://rosettacode.org/wiki/Walk_a_directory/Non-recursively

# [Walk a directory/Non-recursively][1]


The `dir` function takes the directory to traverse, and optionally a named parameter `test`, which is [smart-matched](https://docs.raku.org/routine/$TILDE$TILDE) against the basename of each file (so for example we can use a regex):

```perl
.say for dir ".", :test(/foo/);
```
