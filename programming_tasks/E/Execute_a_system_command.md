[1]: https://rosettacode.org/wiki/Execute_a_system_command

# [Execute a system command][1]

```raku
run "ls" or die $!; # output to stdout
 
my @ls = qx/ls/;    # output to variable
 
my $cmd = 'ls';
@ls = qqx/$cmd/;  # same thing with interpolation
```