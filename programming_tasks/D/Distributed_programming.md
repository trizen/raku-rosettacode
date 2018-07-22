[1]: https://rosettacode.org/wiki/Distributed_programming

# [Distributed programming][1]

Server listens for JSON encoded messages. It processes requests for set|get|dump. 'set' stores a message, 'get' returns message, 'dump' returns all stored messages. Optional parameters for ip address and port.



Server.p6:


#### Output:
```
./server.p6 --usage
Usage:
  server.p6 [--server=<Any>] [--port=<Any>]
```
```perl
#!/usr/bin/env perl6
use JSON::Fast ;
sub MAIN( :$server='0.0.0.0' , :$port=3333 ) {
  my %db ;
  react {
    whenever IO::Socket::Async.listen( $server , $port ) -> $conn {
        whenever $conn.Supply.lines -> $line {
            my %response = 'status' => '' ;
            my $msg = from-json $line ;
            say $msg.perl ;
            given $msg{"function"} {
                when 'set' {
                    %db{ $msg<topic> } = $msg<message> ;
                    %response<status> = 'ok' ;
                }
                when 'get' {
                    %response<topic> = $msg<topic> ;
                    %response<message> = %db{ $msg<topic> } ;
                    %response<status> = 'ok' ;
                }
                when 'dump' {
                    %response = %db ;
                }
                when 'delete' {
                    %db{ $msg<topic> }:delete;
                    %response<status> = 'ok' ;
                }
            }
            $conn.print( to-json(%response, :!pretty) ~ "\n" ) ;
            LAST { $conn.close ; }
            QUIT { default { $conn.close ; say "oh no, $_";}}
            CATCH { default { say .^name, ': ', .Str ,  " handled in $?LINE";}}
        }
    }
  }
}
```


client.p6:


#### Output:
```
Usage:
  client.p6 [--server=<Any>] [--port=<Any>] [--json=<Any>] set <topic> [<message>]
  client.p6 [--server=<Any>] [--port=<Any>] get <topic>
  client.p6 [--server=<Any>] [--port=<Any>] dump
```
```perl
#!/usr/bin/env perl6
use JSON::Fast ;
multi MAIN('set', $topic,  $message='', :$server='localhost', :$port='3333', :$json='') {
    my %msg = function => 'set' , topic=> $topic , message=> $message ;
    %msg{"message"} = from-json( $json ) if $json ;
    sendmsg( %msg , $server, $port) ;
}
multi MAIN('get', $topic, :$server='localhost', :$port='3333') {
    my %msg = function => 'get' , topic=> $topic ;
    sendmsg( %msg , $server, $port) ;
}
multi MAIN('delete', $topic, :$server='localhost', :$port='3333') {
    my %msg = function => 'delete' , topic=> $topic ;
    sendmsg( %msg , $server, $port) ;
}
multi MAIN('dump', :$server='localhost', :$port='3333') {
    my %msg = function => 'dump'  ;
    sendmsg( %msg , $server, $port) ;
}
sub sendmsg( %msg , $server, $port){
    my $conn = await IO::Socket::Async.connect( $server , $port );
    $conn.print: to-json( %msg,:!pretty)~"\n";
    react {
        whenever $conn.Supply -> $data {
            print $data;
            $conn.close;
        }
    }
}
```


examples:


#### Output:
```
echo '{"function":"set","topic":"push","message":["perl5","perl6","rakudo"]}' | nc localhost 3333

./client.p6 set version perl6
{"status": "ok"}
./client.p6 get version
{"status": "ok","topic": "version","message": "perl6"}
./client.p6 --json='["one","two","three"]' set mylist
{"status": "ok"}
./client.p6 dump
{"push": ["perl5","perl6","rakudo"],"version": "perl6","mylist": ["one","two","three"]}
./client.p6 delete version
{"status": "ok"}

server output:
${:function("set"), :message($["perl5", "perl6", "rakudo"]), :topic("push")}
${:function("set"), :message("perl6"), :topic("version")}
${:function("get"), :topic("version")}
${:function("set"), :message($["one", "two", "three"]), :topic("mylist")}
${:function("dump")}
${:function("delete"), :topic("version")}
```