[1]: http://rosettacode.org/wiki/URL_encoding

# [URL encoding][1]

```perl6
my $url = 'http://foo bar/';
 
say $url.subst(/<-alnum>/, *.ord.fmt("%%%02X"), :g);
```

#### Output:
```
http%3A%2F%2Ffoo%20bar%2F
```