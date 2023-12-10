[1]: https://rosettacode.org/wiki/Check_input_device_is_a_terminal

# [Check input device is a terminal][1]



```perl
say $*IN.t ?? "Input comes from tty." !! "Input doesn't come from tty.";
```

#### Output:
```
$ raku istty.raku
Input comes from tty.
$ true | raku istty.raku
Input doesn't come from tty.
```
