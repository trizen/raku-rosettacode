[1]: https://rosettacode.org/wiki/IPC_via_named_pipe

# [IPC via named pipe][1]

Couldn't figure out how to get around the IO blockages as per the talk page so any help would be appreciated, thanks in advance.

```perl
# 20200923 Added Raku programming solution
 
use NativeCall;
use UUID; # cannot mix File::Temp with mkfifo well so use this as tmpnam
 
my ($in, $out) = <in out>.map: { "/tmp/$_-" ~ UUID.new } ;
 
sub mkfifo(Str $pathname, int32 $mode --> int32) is native('libc.so.6') is symbol('mkfifo') {*}
 
($in,$out).map: { die $!.message if mkfifo($_, 0o666) } ; # c style exit code
 
say "In  pipe: ", $in;
say "Out pipe: ", $out;
 
my atomicint $CharCount = 0;
 
signal(SIGINT).tap: {
   ($in,$out).map: { .IO.unlink or die } ;
   say "\nBye." and exit;
}
 
loop {
   given $in.IO.watch { # always true even when nothing is spinning ?
 
      say "Current count: ", $CharCount  ⚛+= $in.IO.open.readchars.codes;
 
      given $out.IO.open(:update :truncate) { # both truncate and flush aren't
         .flush or die ;                      # working on pipes so without a
         .spurt: "$CharCount\t"               # prior consumer it just appends
      }
      $out.IO.open.close or die;
   }
   sleep ½;
}
```


Terminal 1


#### Output:
```
named-pipe.raku
In  pipe: /tmp/in-b49e4cdf-80b6-49da-b4a3-2810f1eeeb6a
Out pipe: /tmp/out-44fcd0db-ce02-47b4-bdd3-34833f3dd621
Current count: 5
Current count: 10
Current count: 15
^C
Bye.
```


Terminal 2


#### Output:
```
echo asdf > /tmp/in-b49e4cdf-80b6-49da-b4a3-2810f1eeeb6a
cat /tmp/out-44fcd0db-ce02-47b4-bdd3-34833f3dd621
5       ^C
echo qwer > /tmp/in-b49e4cdf-80b6-49da-b4a3-2810f1eeeb6a
echo uiop > /tmp/in-b49e4cdf-80b6-49da-b4a3-2810f1eeeb6a
cat /tmp/out-44fcd0db-ce02-47b4-bdd3-34833f3dd621      
10      15      ^C
```
