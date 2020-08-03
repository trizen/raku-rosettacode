[1]: https://rosettacode.org/wiki/Unix/ls

# [Unix/ls][1]

There is a `dir` builtin command which returns a list of IO::Path objects. We stringify them all with a hyperoperator before sorting the strings.

```raku
.say for sort ~«dir
```