[1]: https://rosettacode.org/wiki/Introspection

# [Introspection][1]

```perl
use v6;   # require Perl 6
 
my $bloop = -123;
 
if MY::{'$bloop'}.defined and CORE::{'&abs'}.defined { say abs $bloop }
 
my @ints = ($_ when Int for PROCESS::.values);
say "Number of PROCESS vars of type Int: ", +@ints;
say "PROCESS vars of type Int add up to ", [+] @ints;
```

#### Output:
```
123
Number of PROCESS vars of type Int: 1
PROCESS vars of type Int add up to 28785
```


Obviously Perl 6 doesn't maintain a lot of global integer variables... `:-)`



Nevertheless, you can use similar code to access all the variables in any package you like,
such as the GLOBAL package, which typically has absolutely nothing in it. Since the PROCESS package is even more global than GLOBAL, we used that instead. Go figure...