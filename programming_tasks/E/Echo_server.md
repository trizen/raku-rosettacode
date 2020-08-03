[1]: https://rosettacode.org/wiki/Echo_server

# [Echo server][1]

```perl
my $socket = IO::Socket::INET.new:
    :localhost<localhost>,
    :localport<12321>,
    :listen;
 
while $socket.accept -> $conn {
    say "Accepted connection";
    start {
        while $conn.recv -> $stuff {
            say "Echoing $stuff";
            $conn.print($stuff);
        }
        $conn.close;
    }
}
```


Async version:

```perl
react {
    whenever IO::Socket::Async.listen('0.0.0.0', 12321) -> $conn {
        whenever $conn.Supply.lines -> $line {
            $conn.print( "$line\n" ) ;
        }
    }
}
 
```