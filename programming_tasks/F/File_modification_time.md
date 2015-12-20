[1]: http://rosettacode.org/wiki/File_modification_time

# [File modification time][1]

```perl6
use NativeCall;
 
class utimbuf is repr('CStruct') {
    has int $.actime;
    has int $.modtime;
 
    submethod BUILD(:$atime, :$mtime) {
        $!actime = $atime;
        $!modtime = $mtime;
    }
}
 
sub sysutime(Str, utimbuf --> int) is native is symbol('utime') {*}
 
sub MAIN (Str $file) {
    my $mtime = $file.IO.modified // die "Can't stat $file: $!";
 
    my $ubuff = utimbuf.new(:atime(time),:mtime($mtime));
 
    sysutime($file, $ubuff);
}
```


Sets the last access time to now,
while restoring the modification time to what it was before.