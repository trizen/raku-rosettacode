[1]: https://rosettacode.org/wiki/Multiline_shebang

# [Multiline shebang][1]



```perl
#!/usr/local/bin/raku
use MONKEY; EVAL '(exit $?0)' && EVAL 'exec rake $0 ${1+"$@"}'
& EVAL 'exec raku $0 $argv:q'
        if 0;
```
