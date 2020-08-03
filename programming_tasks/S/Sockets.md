[1]: https://rosettacode.org/wiki/Sockets

# [Sockets][1]

Will fail with a connect error if there is not a socket server of some kind available on the specified host and port.

```raku
my $host = '127.0.0.1';
my $port = 256;
 
my $client = IO::Socket::INET.new(:$host, :$port);
$client.print( 'hello socket world' );
$client.close;
```