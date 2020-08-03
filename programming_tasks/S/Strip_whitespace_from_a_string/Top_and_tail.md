[1]: https://rosettacode.org/wiki/Strip_whitespace_from_a_string/Top_and_tail

# [Strip whitespace from a string/Top and tail][1]

```perl
my $s = "\r\n \t\x2029 Good Stuff \x202F\n";
say $s.trim;
say $s.trim.perl;
say $s.trim-leading.perl;
say $s.trim-trailing.perl;
```

#### Output:
```
Good Stuff
"Good Stuff"
"Good Stuff  \n"
"\r\n \t
 Good Stuff"
```