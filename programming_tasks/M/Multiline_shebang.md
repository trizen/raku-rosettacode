[1]: https://rosettacode.org/wiki/Multiline_shebang

# [Multiline shebang][1]

```raku
#!/usr/local/bin/perl6
use MONKEY; EVAL '(exit $?0)' && EVAL 'exec perl6 $0 ${1+"$@"}'
& EVAL 'exec perl6 $0 $argv:q'
        if 0;
```
