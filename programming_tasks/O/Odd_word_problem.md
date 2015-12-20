[1]: http://rosettacode.org/wiki/Odd_word_problem

# [Odd word problem][1]

A recursive solution, with the added feature that it treats each line separately.

```perl6
my &in = { $*IN.getc // last }
 
loop {
    ew(in);
    ow(in).print;
}
 
multi ew ($_ where /\w/) { .print; ew(in); }
multi ew ($_)            { .print; next when "\n"; }
 
multi ow ($_ where /\w/) { ow(in) x .print; }
multi ow ($_)            { $_; }
```

#### Output:
```
$ ./oddword
we,are;not,in,kansas;any,more.
we,era;not,ni,kansas;yna,more.
what,is,the;meaning,of:life.
what,si,the;gninaem,of:efil.
```


Note how the even/oddness is reset on the line boundary; if not, the second line might have started out in an odd state and reversed "what" instead of "is". The call to <tt>next</tt> prevents that by sending the loop back to its initial state.



There is one clever trick here with the <tt>x</tt> operator; it evaluates both its arguments in order, but in this case only returns the left argument because the right one is always 1 (True). You can think of it as a reversed C-style comma operator.