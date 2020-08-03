[1]: https://rosettacode.org/wiki/Web_scraping

# [Web scraping][1]

```raku
use HTTP::Client; # https://github.com/supernovus/perl6-http-client/
my $site = "http://tycho.usno.navy.mil/cgi-bin/timer.pl";
HTTP::Client.new.get($site).content.match(/'<BR>'( .+? <ws> UTC )/)[0].say
```


Note that the string between '&lt;' and '&gt;' refers to regex tokens, so to match a literal '&lt;BR&gt;' you need to quote it, while &lt;ws&gt; refers to the built-in token whitespace.
Also, whitespace is ignored by default in Perl&#160;6 regexes.