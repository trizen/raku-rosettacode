[1]: http://rosettacode.org/wiki/Get_system_command_output

# [Get system command output][1]

If you don't want to execute it in shell (and you probably don't), then use this:

```perl6
say run($command, $arg1, $arg2, :out).out.slurp-rest;
```


Unfortunately, it is very long to type, but that is the only way to pass your variables as arguments safely.



You might be tempted to start using shell when you have to pipe something, but even in that case there is no need to do so. See this code:

```perl6
my $p1 = run 'echo', 'Hello, world', :out;
my $p2 = run 'cat', '-n', :in($p1.out), :out;
say $p2.out.slurp-rest;
```


See [docs](http://doc.perl6.org/type/Proc) for more info.



If you really want to run something in shell and you understand potential security problems, then you can use `qx//` (interpolates environment variables) or `qqx//` (interpolates normally). See [the docs for more info](http://doc.perl6.org/language/quoting).

```perl6
say qx[dir]
```

#### Output:
```
Find_URI_in_text.p6  History_variables.p6  K-d_tree.pl
Fractran.pl          History_variables.pl  XML_Input.p6
```