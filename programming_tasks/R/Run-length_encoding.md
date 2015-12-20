[1]: http://rosettacode.org/wiki/Run-length_encoding

# [Run-length encoding][1]

This currently depend on a workaround to pass the match object into the replacement closure
as an explicit argument. This is supposed to happen automatically.



Note also that Perl 6 regexes don't care about unquoted whitespace, and that backrefs
count from 0, not from 1.

```perl6
sub encode($str) { $str.subst(/(.) $0*/, -> $/ { $/.chars ~ $0 ~ ' ' }, :g); }
 
sub decode($str) { $str.subst(/(\d+) (.) ' '/, -> $/ {$1 x $0}, :g); }
 
my $e = encode('WWWWWWWWWWWWBWWWWWWWWWWWWBBBWWWWWWWWWWWWWWWWWWWWWWWWBWWWWWWWWWWWWWW');
say $e;
say decode($e);
```


Output:


#### Output:
```
12W 1B 12W 3B 24W 1B 14W 
WWWWWWWWWWWWBWWWWWWWWWWWWBBBWWWWWWWWWWWWWWWWWWWWWWWWBWWWWWWWWWWWWWW
```