[1]: http://rosettacode.org/wiki/Check_output_device_is_a_terminal

# [Check output device is a terminal][1]

The .t method on a filehandle tells you whether it's going to the terminal. Here we use the note function to emit our result to standard error rather than standard out.


#### Output:
```
$ perl6 -e 'note $*OUT.t'
True
$ perl6 -e 'note $*OUT.t' >/dev/null
False
```