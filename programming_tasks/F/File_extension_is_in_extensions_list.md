[1]: http://rosettacode.org/wiki/File_extension_is_in_extensions_list

# [File extension is in extensions list][1]

```perl
sub ext-in-list(@ext,@files) {
    for @files {
        when / :i @ext $ /      { say "True\t$_" }
        when / '.' <-[/.]>+ $ / { say "False\t$_" }
        default                 { say "----\t$_" }
    }
}
 
ext-in-list
    «
        .c
        .C#
        .o
        .yup
        .½xy
       '. embedded blanks '
    »,
    «
        foo.c
        foo.C
        foo.C++
        foo.c#
        foo.zkl
        somefile
       'holy smoke'
        afile.
        /a/path/to/glory.yup/or_not
        funny...
        unusual.½xy
       'fly_in_the_ointment. embedded blanks '
    »;
```

#### Output:
```
True    foo.c
True    foo.C
False   foo.C++
True    foo.c#
False   foo.zkl
----    somefile
----    holy smoke
----    afile.
----    /a/path/to/glory.yup/or_not
----    funny...
True    unusual.½xy
True    fly_in_the_ointment. embedded blanks 
```