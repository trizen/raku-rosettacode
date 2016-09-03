[1]: http://rosettacode.org/wiki/Reflection/Get_source

# [Reflection/Get source][1]

Filename of a subroutine:

```perl
say &sum.file;
```


Filename of a method:

```perl
say Date.^find_method("day-of-week").file;
```


Unfortunately, as of Rakudo 6.c on MoarVM, it prints a precompilation hash (rather than a proper file path) for routines exported by pre-compiled modules, and just `gen/moar/m-CORE.setting` (rather than a full path) for built-ins.