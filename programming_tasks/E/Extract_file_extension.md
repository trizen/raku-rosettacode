[1]: http://rosettacode.org/wiki/Extract_file_extension

# [Extract file extension][1]

```perl
sub extension ( Str $filename --> Str ) {
    given $filename.split(/\./)[*-1] {
        when $filename   { "" }
        when / <[\/_]> / { "" }
        default          { "." ~ $_ }
    }
}
 
say "$_ -> ", extension($_).perl for (
    'mywebsite.com/picture/image.png',
    'http://mywebsite.com/picture/image.png',
    'myuniquefile.longextension',
    'IAmAFileWithoutExtension',
    '/path/to.my/file',
    'file.odd_one',
)
```

#### Output:
```
mywebsite.com/picture/image.png -> ".png"
http://mywebsite.com/picture/image.png -> ".png"
myuniquefile.longextension -> ".longextension"
IAmAFileWithoutExtension -> ""
/path/to.my/file -> ""
file.odd_one -> ""
```