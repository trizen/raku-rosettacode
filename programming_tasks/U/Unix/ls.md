[1]: http://rosettacode.org/wiki/Unix/ls

# [Unix/ls][1]

There is a <tt>dir</tt> builtin command which returns a list of IO::Path objects. We stringify them all with a hyperoperator before sorting the strings.

```perl
.say for sort ~«dir
```