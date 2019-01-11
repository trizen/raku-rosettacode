[1]: https://rosettacode.org/wiki/Copy_stdin_to_stdout

# [Copy stdin to stdout][1]

When invoked at a command line: Slightly less magical than Perl / sed. The p flag means automatically print each line of output to STDOUT. The e flag means execute what follows inside quotes. ".lines" reads lines from the assigned pipe (file handle), STDIN by default.

```perl
perl6 -pe'.lines'
```


When invoked from a file: Lines are auto-chomped, so need to re-add newlines (hence .say rather than .print)

```perl
.say for lines
```