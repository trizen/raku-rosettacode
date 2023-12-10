[1]: https://rosettacode.org/wiki/HTTP

# [HTTP][1]





Using LWP::Simple from [the Raku ecosystem](https://modules.raku.org/search/?q=LWP%3A%3ASimple).

```perl
use LWP::Simple;

print LWP::Simple.get("http://www.rosettacode.org");
```


or, without LWP::Simple:

```perl
my $socket = IO::Socket::INET.new(host => "www.rosettacode.org",
				  port => 80,);
$socket.print("GET / HTTP/1.0\r\n\r\n");
print $socket.recv();
$socket.close;
```
