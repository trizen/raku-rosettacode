[1]: https://rosettacode.org/wiki/Parse_command-line_arguments

# [Parse command-line arguments][1]


At the end of running any top-level code (which can preprocess the arguments if it likes), Raku automatically examines any remaining arguments and transforms them into a call to a `MAIN` routine, if one is defined.  The arguments are parsed based on the signature of the routine, so that options are mapped to named arguments.

```perl
sub MAIN (Bool :$b, Str :$s = '', Int :$n = 0, *@rest) {
    say "Bool: $b";
    say "Str: $s";
    say "Num: $n";
    say "Rest: @rest[]";
}
```

#### Output:
```
$ ./main -h
Usage:
  ./main [-b] [-s=<Str>] [-n=<Int>] [<rest> ...]

$ ./main -b -n=42 -s=turtles all the way down
Bool: True
Str: turtles
Num: 42
Rest: all the way down
```


If there are multiple `MAIN` subs, they are differentiated by multiple dispatch.  A help message can automatically be generated for all the variants.  The intent of this mechanism is not to cover every possible switch structure, but just to make it drop-dead easy to handle most of the common ones.
