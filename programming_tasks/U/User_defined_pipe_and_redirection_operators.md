[1]: https://rosettacode.org/wiki/User_defined_pipe_and_redirection_operators

# [User defined pipe and redirection operators][1]


Implementing cat, redirect(&lt;), head, tail and tee. I share the same general lack of enthusiasm as the Perl example author to implement the other shell ops. Many of the ops already exist in some form in Raku, though they may have slightly different syntax and precedence as the shell ops. I didn't bother implementing pipe (|) as Raku pretty much has chaining semantics built in.



The less than '&lt;' operator is very low level in Raku and would be troublesome to override, so is implemented as 'redirect'. The sort and unique directives don't really have any effect on the final result string, it would be the same without them, but it is following the task example.



This assumes that the data is in a tab separated file "List\_of\_computer\_scientists.lst" in the current directory.

```perl
sub cat ($fname) { lazy $fname.IO.lines };
sub head ($count, @positional) { @positional.head($count) }
sub redirect ($fname) { cat($fname) }
sub tail (Int $count, Str $fname) { $fname.IO.lines.tail($count) }
sub infix:<tee> ($stream, $fname) {
    # need to reify the lazy list to write to a file
    my @values = @$stream[^Inf].grep: *.defined;
    # meh. not really going to write the files
    # $fname.IO.put @values.join("\n")
    # just pass the reified values along
    @values.List
}

my $aa = ((
    (head 4, redirect 'List_of_computer_scientists.lst'),
    cat('List_of_computer_scientists.lst') .grep( /'ALGOL'/ ) tee 'ALGOL_pioneers.lst',
    tail 4, 'List_of_computer_scientists.lst'
).flat .sort .unique tee 'the_important_scientists.lst') .grep: /'aa'/;

say "Pioneer: $aa";
```

#### Output:
```
Pioneer: Adriaan van Wijngaarden        Dutch pioneer; ARRA, ALGOL
```
