[1]: http://rosettacode.org/wiki/Check_input_device_is_a_terminal

# [Check input device is a terminal][1]

```perl
say $*IN.t ?? "Input comes from tty." !! "Input doesn't come from tty.";
```

#### Output:
```
$ perl6 istty.p6
Input comes from tty.
$ true | perl6 istty.p6
Input doesn't come from tty.
```