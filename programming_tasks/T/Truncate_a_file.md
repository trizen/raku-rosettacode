[1]: https://rosettacode.org/wiki/Truncate_a_file

# [Truncate a file][1]

```perl
use NativeCall;
 
sub truncate(Str, int32 --> int32) is native {*}
 
sub MAIN (Str $file, Int $to) {
    given $file.IO { 
        .e        or die "$file doesn't exist";
        .w        or die "$file isn't writable";
        .s >= $to or die "$file is not big enough to truncate";
    }
    truncate($file, $to) == 0 or die "Truncation was unsuccessful";
}
```