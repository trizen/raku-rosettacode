[1]: http://rosettacode.org/wiki/Start_from_a_main_routine

# [Start from a main routine][1]

When executed with the standard setting, Perl 6 code always runs the mainline code automatically, followed by the <tt>MAIN</tt> function if you have one. However, it's possible to start up with an alternate setting that might want to create its own event loop or <tt>MAIN</tt>. In such cases you can
always capture control at various phases with blocks that we call "phasers":

```perl6
BEGIN {...} # as soon as parsed
CHECK {...} # end of compile time
INIT {...}  # beginning of run time
END {...}   # end of run time
```