[1]: https://rosettacode.org/wiki/Command-line_arguments

# [Command-line arguments][1]


Perl 5's `@ARGV` is available as `@*ARGS`. Alternatively, if you define a subroutine named `MAIN`, Perl will automatically process `@*ARGS` according to Unix conventions and `MAIN`'s signature (or signatures, if your `MAIN` is a multi sub) and then call `MAIN` with appropriate arguments; for more detailed information see:  [https://docs.raku.org/language/create-cli](https://docs.raku.org/language/create-cli)

```perl
# with arguments supplied
$ raku -e 'sub MAIN($x, $y) { say $x + $y }' 3 5
8

# missing argument:
$ raku -e 'sub MAIN($x, $y) { say $x + $y }' 3 
Usage:
-e '...' x y
```


If the program is stored in a file, the file name is printed instead of `-e '...'`.
