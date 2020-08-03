[1]: https://rosettacode.org/wiki/File_extension_is_in_extensions_list

# [File extension is in extensions list][1]

Does the extra credit requirement.

```raku
sub check-extension ($filename, *@extensions) {
    so $filename ~~ /:i '.' @extensions $/
}
 
# Testing:
 
my @extensions = <zip rar 7z gz archive A## tar.bz2>;
my @files= <
    MyData.a##  MyData.tar.Gz  MyData.gzip  MyData.7z.backup  MyData...  MyData
    MyData_v1.0.tar.bz2  MyData_v1.0.bz2
>;
say "{$_.fmt: '%-19s'} - {check-extension $_, @extensions}" for @files;
```

#### Output:
```
MyData.a##          - True
MyData.tar.Gz       - True
MyData.gzip         - False
MyData.7z.backup    - False
MyData...           - False
MyData              - False
MyData_v1.0.tar.bz2 - True
MyData_v1.0.bz2     - False
```