[1]: https://rosettacode.org/wiki/Extract_file_extension

# [Extract file extension][1]

The built-in `IO::Path` class has an `.extension` method:

```perl
say $path.IO.extension;
```


Contrary to this task's specification, it





Here's a custom implementation which does satisfy the task requirements:

```perl
sub extension (Str $path --> Str) {
    $path.match(/:i ['.' <[a..z0..9]>+]? $ /).Str
}
 
# Testing:
 
printf "%-35s %-11s %-12s\n", $_, extension($_).perl, $_.IO.extension.perl
for <
    http://example.com/download.tar.gz
    CharacterModel.3DS
    .desktop
    document
    document.txt_backup
    /etc/pam.d/login
>;
```

#### Output:
```
http://example.com/download.tar.gz  ".gz"       "gz"        
CharacterModel.3DS                  ".3DS"      "3DS"       
.desktop                            ".desktop"  "desktop"   
document                            ""          ""          
document.txt_backup                 ""          "txt_backup"
/etc/pam.d/login                    ""          ""
```