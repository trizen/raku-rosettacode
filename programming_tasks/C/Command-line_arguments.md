[1]: https://rosettacode.org/wiki/Command-line_arguments

# [Command-line arguments][1]

Perl 5's `@ARGV` is available as `@*ARGS`. Alternatively, if you define a subroutine named `MAIN`, Perl will automatically process `@*ARGS` according to Unix conventions and `MAIN`'s signature (or signatures, if your `MAIN` is a multi sub) and then call `MAIN` with appropriate arguments; see [Synopsis 6](http://perlcabal.org/syn/S06.html#Declaring_a_MAIN_subroutine) or [[1]](http://perlgeek.de/en/article/5-to-6#post_14%7C5-to-6).

```raku
# with arguments supplied
$ perl6 -e 'sub MAIN($x, $y) { say $x + $y }' 3 5
8
 
# missing argument:
$ perl6 -e 'sub MAIN($x, $y) { say $x + $y }' 3 
Usage:
-e '...' x y
```


If the program is stored in a file, the file name is printed instead of `-e '...'`.