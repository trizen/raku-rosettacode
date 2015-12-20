[1]: http://rosettacode.org/wiki/Echo_server

# [Echo server][1]

```perl6
my $socket = IO::Socket::INET.new:
    localhost => 'localhost',
    localport => 12321,
    listen    => 1;
 
while $socket.accept -> $conn {
    say "Accepted connection";
    start {
        while $conn.recv -> $stuff {
            say "Echoing $stuff";
            $conn.send($stuff);
        }
        $conn.close;
    }
}
```