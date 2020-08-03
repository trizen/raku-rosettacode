[1]: https://rosettacode.org/wiki/Reflection/Get_source

# [Reflection/Get source][1]

A full path is provided for built-in routines/methods. However for routines exported by pre-compiled modules a precompilation hash is returned, not a proper file path.

```raku
say &sum.file;
say Date.^find_method("day-of-week").file;
```

#### Output:
```
SETTING::src/core/Any.pm
SETTING::src/core/Dateish.pm
```