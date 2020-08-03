[1]: https://rosettacode.org/wiki/Print_itself

# [Print itself][1]

Not really sure what the point of this task is.



Is it supposed to be a quine?

```raku
my &f = {say $^s, $^s.raku;}; f "my \&f = \{say \$^s, \$^s.raku;}; f "
 
```


Or just a program that when executed echoes its source to STDOUT? (Here's probably the simplest valid program that when executed, echoes its source to STDOUT. It is exceptionally short: zero bytes; and when executed echoes zero bytes to STDOUT.)

```raku
 
```


Or are we supposed to demonstrate how to locate the currently executing source code file and incidentally, print it.

```raku
print $*PROGRAM.slurp
```


Whatever. Any of these satisfy the rather vague specifications.
