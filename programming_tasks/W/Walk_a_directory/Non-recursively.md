[1]: http://rosettacode.org/wiki/Walk_a_directory/Non-recursively

# [Walk a directory/Non-recursively][1]

The `dir` function takes the directory to traverse, and optionally a named parameter `test`, which can for example be a regex:

```perl6
.say for dir(".", :test(/foo/))
```