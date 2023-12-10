[1]: https://rosettacode.org/wiki/File_modification_time

# [File modification time][1]



```perl
use NativeCall;

class utimbuf is repr('CStruct') {
    has int $.actime;
    has int $.modtime;

    submethod BUILD(:$atime, :$mtime) {
        $!actime = $atime;
        $!modtime = $mtime.to-posix[0].round;
    }
}

sub sysutime(Str, utimbuf --> int32) is native is symbol('utime') {*}

sub MAIN (Str $file) {
    my $mtime = $file.IO.modified orelse .die;

    my $ubuff = utimbuf.new(:atime(time),:mtime($mtime));

    sysutime($file, $ubuff);
}
```


Sets the last access time to now, 
while restoring the modification time to what it was before.
