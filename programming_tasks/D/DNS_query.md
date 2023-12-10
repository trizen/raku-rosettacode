[1]: https://rosettacode.org/wiki/DNS_query

# [DNS query][1]



```perl
use Net::DNS;

my $resolver = Net::DNS.new('8.8.8.8');

my $ip4 = $resolver.lookup('A',    'orange.kame.net');
my $ip6 = $resolver.lookup('AAAA', 'orange.kame.net');

say $ip4[0].octets.join: '.';
say $ip6[0].octets.».fmt("%.2X").join.comb(4).join: ':';
```

#### Output:
```
203.178.141.194
2001:0200:0dff:fff1:0216:3eff:feb1:44d7
```
