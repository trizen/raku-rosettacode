[1]: https://rosettacode.org/wiki/Hello_world/Web_server

# [Hello world/Web server][1]

```perl
my $listen = IO::Socket::INET.new(:listen, :localhost<localhost>, :localport(8080));
loop {
    my $conn = $listen.accept;
    my $req =  $conn.get ;
    $conn.print: "HTTP/1.0 200 OK\r\nContent-Type: text/plain; charset=UTF-8\r\n\r\nGoodbye, World!\r\n";
    $conn.close;
}
```


Async:

```perl
react {
    whenever IO::Socket::Async.listen('0.0.0.0', 8080) -> $conn {
        whenever $conn.Supply.lines -> $line {
            $conn.print: "HTTP/1.0 200 OK\r\nContent-Type: text/plain; charset=UTF-8\r\n\r\nGoodbye, World!\r\n";
            $conn.close;
        }
    }
}
```