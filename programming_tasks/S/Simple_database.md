[1]: https://rosettacode.org/wiki/Simple_database

# [Simple database][1]

A generic client/server JSON database.
server.p6:

```raku
#!/usr/bin/env perl6
use JSON::Fast ;
sub MAIN( :$server='0.0.0.0', :$port=3333, :$dbfile='db' ) {
    my %db;
    my %index;
    my $dbdata = slurp "$dbfile.json" ;
    my $indexdata = slurp "{$dbfile}_index.json" ;
    %db = from-json($dbdata) if $dbdata ;
    %index = from-json($indexdata) if $indexdata ;
  react {
    whenever IO::Socket::Async.listen( $server , $port ) -> $conn {
        whenever $conn.Supply.lines -> $line {
            my %response = 'status' => '' ;
            my $msg = from-json $line ;
            say $msg.perl ;
            given $msg<function> { 
                when 'set' {
                    %db{ $msg<topic> } = $msg<message> ;
                    %response<status> = 'ok' ;
                    %index<last_> = $msg<topic> ;
                    for %index<keys_>.keys -> $key {
                        if $msg<message>{$key} {
                            %index<lastkey_>{ $key }{ $msg<message>{$key} } = $msg<topic> ;    
                            %index<idx_>{ $key }{ $msg<message>{$key} }{ $msg<topic> } = 1 ;
                        }
                    }
                    spurt "$dbfile.json", to-json(%db);
                    spurt "{$dbfile}_index.json", to-json(%index);
                }
                when 'get' {
                    %response<topic> = $msg<topic> ;
                    %response<message> = %db{ $msg<topic> } ;
                    %response<status> = 'ok' ;
                }
                when 'dump' {
                    %response{'data'} = %db ;
                    %response<status> = 'ok' ;
                }
                when 'dumpindex' {
                    %response{'data'} = %index ;
                    %response<status> = 'ok' ;
                }
                when 'delete' {
                    %db{ $msg<topic> }:delete;
                    %response<status> = 'ok' ;
                    spurt "$dbfile.json", to-json(%db);
                    reindex();
                }
                when 'addindex' {
                    %response<status> = 'ok' ;
                    %index<keys_>{ $msg<key>} =1 ;
                    reindex();
                }
                when 'reportlast' {
                    %response{'data'} = %db{%index<last_>} ;
                    %response<status> = 'ok' ;
                }
                when 'reportlastindex' {
                    %response<key> = $msg<key> ;
                    for %index<lastkey_>{$msg<key>}.keys -> $value {
                        #%response{'data'}.push: %db{ %index<lastkey_>{ $msg<key>  }{ $value } } ;
                        %response{'data'}{$value} = %db{ %index<lastkey_>{ $msg<key>  }{ $value } } ;
                    }
                    %response<status> = 'ok' ;
                }
                when 'reportindex' {
                    %response<status> = 'ok' ;
                    for %index<idx_>{$msg<key>}.keys.sort -> $value  {
                        for %index<idx_>{ $msg<key> }{ $value }.keys.sort -> $topic {
                            %response<data>.push:  %db{ $topic }  ;
                            #%response<data>{$value} = %db{ $topic }  ;
                        }
                    }
                }
                when 'commit' {
                    spurt "$dbfile.json", to-json(%db);
                    spurt "{$dbfile}_index.json", to-json(%index);
                    %response<status> = 'ok' ;
                }
                default {
                    %response<status> = 'error';
                    %response<error> = 'no function or not supported';
                }
            }
            $conn.print( to-json(%response, :!pretty) ~ "\n" ) ;
            LAST { $conn.close ; }
            QUIT { default { $conn.close ; say "oh no, $_";}}
            CATCH { default { say .^name, ': ', .Str ,  " handled in $?LINE";}}
        }
    }
  }
    sub reindex {
        %index<idx_>:delete;
        for %db.keys -> $topic {
            my $msg = %db{$topic} ;
            for %index<keys_>.keys -> $key {
                if $msg{$key} {
                    %index<idx_>{ $key }{ $msg{$key} }{ $topic } = 1 ;
                }
            }
        }
        spurt "{$dbfile}_index.json", to-json(%index) ;
    }
}
```


client.p6

```raku
#!/usr/bin/env perl6
use JSON::Fast ;
multi MAIN('set', $topic,  $message='', :$server='localhost', :$port='3333', :$json='') {
    my %msg = function => 'set' , topic=> $topic , message=> $message ;
    %msg{"message"} = from-json( $json ) if $json ;
    sendmsg( %msg , $server, $port) ;
}
multi MAIN('add', $topic,  $json, :$server='localhost', :$port='3333' ) {
    my %msg = function => 'set' , topic=> $topic;
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
multi MAIN('addindex', $key, :$server='localhost', :$port='3333') {
    my %msg = function => 'addindex', key => $key  ;
    sendmsg( %msg , $server, $port) ;
}
multi MAIN('reportindex', $key, :$server='localhost', :$port='3333') {
    my %msg = function => 'reportindex', key => $key  ;
    sendmsg( %msg , $server, $port) ;
}
multi MAIN('reportlastindex', $key, :$server='localhost', :$port='3333') {
    my %msg = function => 'reportlastindex', key => $key  ;
    sendmsg( %msg , $server, $port) ;
}
multi MAIN('reportlast', :$server='localhost', :$port='3333') {
    my %msg = function => 'reportlast' ;
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


Example:


#### Output:
```
./client.p6 addindex constructor
./client.p6 addindex date

./client.p6 add 2007 '{"date":"2007-11-04","constructor":"Ducati","name":"Casey Stoner"}'
./client.p6 add 2008 '{"date":"2008-10-26","constructor":"Yamaha","name":"Valentino Rossi"}'
./client.p6 add 2009 '{"date":"2009-11-08","constructor":"Yamaha","name":"Valentino Rossi"}'
./client.p6 add 2010 '{"date":"2010-11-17","constructor":"Yamaha","name":"Jorge Lorenzo"}'
./client.p6 add 2011 '{"date":"2011-11-06","constructor":"Honda","name":"Casey Stoner"}'
./client.p6 add 2012 '{"date":"2012-11-11","constructor":"Yamaha","name":"Jorge Lorenzo"}'
./client.p6 add 2013 '{"date":"2013-11-10","constructor":"Honda","name":"Marc Márquez"}'
./client.p6 add 2014 '{"date":"2014-11-09","constructor":"Honda","name":"Marc Márquez"}'
./client.p6 add 2015 '{"date":"2015-11-08","constructor":"Yamaha","name":"Jorge Lorenzo"}'
./client.p6 add 2016 '{"date":"2016-11-13","constructor":"Honda","name":"Marc Márquez"}'
./client.p6 add 2017 '{"date":"2017-11-12","constructor":"Honda","name":"Marc Márquez"}'

./client.p6 reportlast
./client.p6 reportlastindex constructor
./client.p6 reportindex date
```