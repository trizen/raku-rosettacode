[1]: http://rosettacode.org/wiki/Interactive_programming

# [Interactive programming][1]

Using [Rakudo](http://rosettacode.org/wiki/Rakudo).

```perl
$ rakudo/perl6
> sub f($str1,$str2,$sep) { $str1~$sep x 2~$str2 };
f
> f("Rosetta","Code",":");
Rosetta::Code
>
 
```