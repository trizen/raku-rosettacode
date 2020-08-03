[1]: https://rosettacode.org/wiki/HTTPS

# [HTTPS][1]

There are several modules that provide HTTPS capability. WWW and HTTP::UserAgent are probably the most popular right now, but others exist.

```raku
use WWW;
say get 'https://sourceforge.net/';
```


or

```raku
use HTTP::UserAgent;
say HTTP::UserAgent.new.get('https://sourceforge.net/').content;
```