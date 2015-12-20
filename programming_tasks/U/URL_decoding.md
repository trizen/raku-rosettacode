[1]: http://rosettacode.org/wiki/URL_decoding

# [URL decoding][1]

```perl6
my $url = "http%3A%2F%2Ffoo%20bar%2F";
 
say $url.subst: :g,
    /'%'(<:hexdigit>**2)/,
    ->  ($ord          ) { chr(:16(~$ord)) }
```


Alternately, you can use an in-place substitution:

```perl6
$url ~~ s:g[ '%' (<:hexdigit> ** 2) ] = chr :16(~$0);
say $url;
```