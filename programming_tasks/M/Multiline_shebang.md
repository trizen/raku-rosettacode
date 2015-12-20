[1]: http://rosettacode.org/wiki/Multiline_shebang

# [Multiline shebang][1]

```perl
#!/usr/local/bin/perl6
eval '(exit $?0)' && eval 'exec perl6 $0 ${1+"$@"}'
& eval 'exec perl6 $0 $argv:q'
        if 0;
```