[1]: https://rosettacode.org/wiki/URL_parser

# [URL parser][1]

Uses the URI library which implements a Perl 6 grammar based on the [RFC 3986](https://rosettacode.org//tools.ietf.org/html/rfc3986) BNF grammar.

```raku
use URI;

my @test-uris = <
    foo://example.com:8042/over/there?name=ferret#nose
    urn:example:animal:ferret:nose
    jdbc:mysql://test_user:ouupppssss@localhost:3306/sakila?profileSQL=true
    ftp://ftp.is.co.za/rfc/rfc1808.txt
    http://www.ietf.org/rfc/rfc2396.txt#header1
    ldap://[2001:db8::7]/c=GB?objectClass?one
    mailto:John.Doe@example.com
    news:comp.infosystems.www.servers.unix
    tel:+1-816-555-1212
    telnet://192.0.2.16:80/
    urn:oasis:names:specification:docbook:dtd:xml:4.1.2
>;

my $u = URI.new;

for @test-uris -> $uri {
    say "URI:\t", $uri;
        $u.parse($uri);
        for <scheme host port path query frag> -> $t {
           my $token = try {$u."$t"()} || '';
           say "$t:\t", $token if $token;
        }
    say '';
}
```

#### Output:
```
URI:    foo://example.com:8042/over/there?name=ferret#nose
scheme: foo
host:   example.com
port:   8042
path:   /over/there
query:  name=ferret
frag:   nose

URI:    urn:example:animal:ferret:nose
scheme: urn
path:   example:animal:ferret:nose

URI:    jdbc:mysql://test_user:ouupppssss@localhost:3306/sakila?profileSQL=true
scheme: jdbc
path:   mysql://test_user:ouupppssss@localhost:3306/sakila
query:  profileSQL=true

URI:    ftp://ftp.is.co.za/rfc/rfc1808.txt
scheme: ftp
host:   ftp.is.co.za
port:   21
path:   /rfc/rfc1808.txt

URI:    http://www.ietf.org/rfc/rfc2396.txt#header1
scheme: http
host:   www.ietf.org
port:   80
path:   /rfc/rfc2396.txt
frag:   header1

URI:    ldap://[2001:db8::7]/c=GB?objectClass?one
scheme: ldap
host:   [2001:db8::7]
port:   389
path:   /c=GB
query:  objectClass?one

URI:    mailto:John.Doe@example.com
scheme: mailto
path:   John.Doe@example.com

URI:    news:comp.infosystems.www.servers.unix
scheme: news
port:   119
path:   comp.infosystems.www.servers.unix

URI:    tel:+1-816-555-1212
scheme: tel
path:   +1-816-555-1212

URI:    telnet://192.0.2.16:80/
scheme: telnet
host:   192.0.2.16
port:   80
path:   /

URI:    urn:oasis:names:specification:docbook:dtd:xml:4.1.2
scheme: urn
path:   oasis:names:specification:docbook:dtd:xml:4.1.2
```
