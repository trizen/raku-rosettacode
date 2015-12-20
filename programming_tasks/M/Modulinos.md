[1]: http://rosettacode.org/wiki/Modulinos

# [Modulinos][1]

Perl 6 automatically calls MAIN on direct invocation, but this may be a multi dispatch, so a library may have multiple "scripted mains".

```perl6
class LUE {
    has $.answer = 42;
}
 
multi MAIN ('test') {
    say "ok" if LUE.new.answer == 42;
}
 
multi MAIN ('methods') {
    say ~LUE.^methods;
}
```