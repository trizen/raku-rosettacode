[1]: https://rosettacode.org/wiki/Web_scraping

# [Web scraping][1]



```perl
# 20210301 Updated Raku programming solution

use HTTP::Client; # https://github.com/supernovus/perl6-http-client/

#`[ Site inaccessible since 2019 ?
my $site = "http://tycho.usno.navy.mil/cgi-bin/timer.pl";
HTTP::Client.new.get($site).content.match(/'<BR>'( .+? <ws> UTC )/)[0].say
# ]

my $site = "https://www.utctime.net/";
my $matched = HTTP::Client.new.get($site).content.match(
   /'<td>UTC</td><td>'( .*Z )'</td>'/
)[0];

say $matched;
#$matched = '12321321:412312312 123';
with DateTime.new($matched.Str) {
   say 'The fetch result seems to be of a valid time format.'
} else {
   CATCH { put .^name, ': ', .Str }
}
```


Note that the string between '&lt;' and '&gt;' refers to regex tokens, so to match a literal '&lt;BR&gt;' you need to quote it, while &lt;ws&gt; refers to the built-in token whitespace.
Also, whitespace is ignored by default in Raku regexes.


```
｢2021-03-01T17:02:37Z｣
The fetch result seems to be of a valid time format.
```
