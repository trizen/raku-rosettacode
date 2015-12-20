[1]: http://rosettacode.org/wiki/HTTP

# [HTTP][1]

```perl6
 
use v6;
# Using LWP::Simple from: git://github.com/cosimo/perl6-lwp-simple.git
use LWP::Simple;
 
print LWP::Simple.get("http://www.rosettacode.org");
 
```


or, without LWP::Simple:

```perl6
 
use v6;
 
my $socket = IO::Socket::INET.new(host => "www.rosettacode.org",
				  port => 80,);
$socket.print("GET / HTTP/1.0\r\n\r\n");
print $socket.recv();
$socket.close;
 
```