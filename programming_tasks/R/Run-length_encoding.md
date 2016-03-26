[1]: http://rosettacode.org/wiki/Run-length_encoding

# [Run-length encoding][1]

Note that Perl 6 regexes don't care about unquoted whitespace, and that backrefs
count from 0, not from 1.

```perl
sub encode($str) { $str.subst(/(.) $0*/, { $/.chars ~ $0 }, :g) }
 
sub decode($str) { $str.subst(/(\d+) (.)/, { $1 x $0 }, :g) }
 
my $e = encode('WWWWWWWWWWWWBWWWWWWWWWWWWBBBWWWWWWWWWWWWWWWWWWWWWWWWBWWWWWWWWWWWWWW');
say $e;
say decode($e);
```


Output:


#### Output:
```
12W1B12W3B24W1B14W 
WWWWWWWWWWWWBWWWWWWWWWWWWBBBWWWWWWWWWWWWWWWWWWWWWWWWBWWWWWWWWWWWWWW
```